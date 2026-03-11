# Architecture Diagram

PhotoAlbum is an ASP.NET Core 9.0 Razor Pages web application for photo gallery management, using Entity Framework Core with SQL Server for persistence and local file system storage for uploaded images.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nUser / Client"]

    subgraph Web["ASP.NET Core 9.0 Web Application"]
        Pages["Razor Pages\n(Index, Detail, PhotoFile, Privacy)"]
        Service["IPhotoService / PhotoService\nUpload validation, image processing,\nfile storage orchestration"]
        EF["Entity Framework Core 9.0\n(DbContext - PhotoAlbumContext)"]
    end

    subgraph Storage["Data Storage"]
        DB["SQL Server LocalDB\nPhotoAlbumDb\n(Photo metadata)"]
        FS["Local File System\nwwwroot/uploads\n(Image files - JPEG, PNG, GIF, WebP)"]
    end

    subgraph Libs["Key Libraries"]
        ImageSharp["SixLabors.ImageSharp 3.x\nImage dimension extraction"]
    end

    Browser -->|"HTTP requests"| Pages
    Pages -->|"Upload / retrieve photos"| Service
    Service -->|"Persist metadata"| EF
    EF -->|"SQL queries"| DB
    Service -->|"Read / write image files"| FS
    Service -->|"Extract image dimensions"| ImageSharp
    Pages -->|"Serve image files"| FS
```
