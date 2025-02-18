# SY.EntityFrameworkCore.Pagination

## Overview
EntityFrameworkCore.Pagination is a lightweight and efficient pagination library for .NET 8.0 applications using Entity Framework Core. It simplifies paginated data retrieval by providing an easy-to-use extension method.

## Installation
To install the package, run the following command:
```sh
dotnet add package EntityFrameworkCore.Pagination
```

## Usage
Use the `ToPagedListAsync` extension method to paginate your queries:
```csharp
var products = await _context.Products
                .ToPagedListAsync(pageNumber, pageSize);
```

### Example Result
The result will be returned as a `PaginationResult<T>` object:
```json
{
  "datas": [
    {
      "id": 1,
      "name": "Product",
      "price": 10
    }
  ],
  "pageNumber": 1,
  "pageSize": 15,
  "totalPages": 67,
  "totalCount": 670,
  "isFirstPage": true,
  "isLastPage": false
}
```

## Methods
This library includes one extension method and one helper class:

### `ToPagedListAsync<T>`
```csharp
public static async Task<PaginationResult<T>> ToPagedListAsync<T>(
    this IQueryable<T> source,
    int pageNumber,
    int pageSize,
    CancellationToken cancellationToken = default)
    where T : class
```
#### Description
This method takes an `IQueryable<T>` source and paginates it using the given `pageNumber` and `pageSize`.
- **Parameters:**
  - `source`: The queryable source.
  - `pageNumber`: The current page number.
  - `pageSize`: The number of records per page.
  - `cancellationToken` (optional): A cancellation token.
- **Returns:** A `PaginationResult<T>` containing paginated data and metadata.

### `PaginationResult<T>` Class
```csharp
public class PaginationResult<T>
{
    public PaginationResult(IList<T> datas, int pageNumber, int pageSize, int totalCount)
    {
        Datas = datas;
        PageNumber = pageNumber;
        PageSize = pageSize;
        TotalPages = totalCount > 0 ? (int)Math.Ceiling(totalCount / (double)pageSize) : 0;
        TotalCount = totalCount;
        IsFirstPage = PageNumber == 1;
        IsLastPage = PageNumber == TotalPages;
    }

    public IList<T> Datas { get; set; }
    public int PageNumber { get; set; }
    public int PageSize { get; set; }
    public int TotalPages { get; set; }
    public int TotalCount { get; set; }
    public bool IsFirstPage { get; set; }
    public bool IsLastPage { get; set; }
}
```
#### Description
This class represents the paginated data result.
- **Properties:**
  - `Datas`: The list of records in the current page.
  - `PageNumber`: The current page number.
  - `PageSize`: The number of records per page.
  - `TotalPages`: The total number of pages.
  - `TotalCount`: The total number of records.
  - `IsFirstPage`: Indicates if this is the first page.
  - `IsLastPage`: Indicates if this is the last page.

## License
This project is open-source and available under the MIT License.

