# Architecture Diagram

PhotoAlbum is an ASP.NET Core 9.0 Razor Pages web application for photo gallery management, using SQL Server LocalDB for metadata persistence and local file system storage for images.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nHTTP Client"]

    subgraph WebApp["ASP.NET Core 9.0 Web Application"]
        subgraph Pages["Presentation Layer - Razor Pages"]
            Index["Index.cshtml\nGallery Grid + Upload"]
            Detail["Detail.cshtml\nFull-size View + Metadata"]
            PhotoFile["PhotoFile.cshtml\nFile Retrieval Endpoint"]
        end

        subgraph Services["Service Layer"]
            IPhotoService["IPhotoService\nInterface"]
            PhotoService["PhotoService\nUpload, Validate, Store, Delete"]
        end

        subgraph Data["Data Access Layer - EF Core 9.0"]
            DbContext["PhotoAlbumContext\nDbContext"]
        end

        subgraph Config["Configuration"]
            AppSettings["appsettings.json\nMax 10MB, JPEG/PNG/GIF/WebP"]
        end
    end

    subgraph Storage["Storage"]
        SQLServer["SQL Server LocalDB\nPhotoAlbumDb\nPhoto metadata"]
        FileSystem["Local File System\nwwwroot/uploads\nGUID-named image files"]
    end

    subgraph Libraries["Key Libraries"]
        ImageSharp["SixLabors.ImageSharp 3.1\nImage dimension extraction"]
    end

    Browser -- "HTTP Requests" --> Pages
    Pages -- "Calls" --> IPhotoService
    IPhotoService -- "Implemented by" --> PhotoService
    PhotoService -- "Persists metadata" --> DbContext
    PhotoService -- "Reads image dimensions" --> ImageSharp
    PhotoService -- "Stores image files" --> FileSystem
    DbContext -- "EF Core SQL Server" --> SQLServer
    Config -- "Configures" --> PhotoService
```
