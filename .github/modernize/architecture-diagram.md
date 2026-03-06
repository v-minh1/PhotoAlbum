# Architecture Diagram

PhotoAlbum is an ASP.NET Core 9.0 Razor Pages web application for photo gallery management, using SQL Server LocalDB for metadata persistence and local file storage for images.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nUser Interface"]

    subgraph App["ASP.NET Core 9.0 Web Application"]
        subgraph Pages["Presentation Layer\n(Razor Pages)"]
            Index["Index.cshtml\nGallery Grid + Upload"]
            Detail["Detail.cshtml\nPhoto Detail + Navigation"]
            PhotoFile["PhotoFile.cshtml\nFile Retrieval Endpoint"]
        end

        subgraph Services["Service Layer"]
            IPhotoService["IPhotoService\nInterface"]
            PhotoService["PhotoService\nUpload / Retrieve / Delete\nValidation + Image Processing"]
        end

        subgraph Data["Data Access Layer\n(Entity Framework Core 9.0)"]
            Context["PhotoAlbumContext\nDbContext"]
            Photo["Photo Model\nMetadata + Dimensions"]
        end
    end

    subgraph Storage["Storage"]
        SQLDB["SQL Server LocalDB\nPhoto Metadata"]
        FileSystem["Local File System\nwwwroot/uploads\nGUID-named image files"]
    end

    ImageSharp["SixLabors.ImageSharp\nImage Dimension Extraction"]

    Browser -->|"HTTP Requests"| Pages
    Pages -->|"Calls"| IPhotoService
    IPhotoService --> PhotoService
    PhotoService -->|"Persists metadata"| Context
    PhotoService -->|"Stores image files"| FileSystem
    PhotoService -->|"Extracts dimensions"| ImageSharp
    Context -->|"SQL queries"| SQLDB
    PhotoFile -->|"Serves files"| FileSystem
```
