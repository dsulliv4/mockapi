[ApiController]
[Route("api/[controller]")]
public class BooksController : ControllerBase
{
    private readonly List<Book> _books = new List<Book>
    {
        new Book { Id = 1, Title = "Book 1", Author = "Author 1" },
        new Book { Id = 2, Title = "Book 2", Author = "Author 2" },
    };

    [HttpGet]
    public ActionResult<IEnumerable<Book>> Get()
    {
        return _books;
    }

    [HttpGet("{id}")]
    public ActionResult<Book> Get(int id)
    {
        var book = _books.FirstOrDefault(b => b.Id == id);
        if (book == null)
        {
            return NotFound();
        }

        return book;
    }

    [HttpPost]
    public ActionResult<Book> Post([FromBody] Book book)
    {
        book.Id = _books.Max(b => b.Id) + 1;
        _books.Add(book);
        return CreatedAtAction(nameof(Get), new { id = book.Id }, book);
    }

    [HttpPut("{id}")]
    public IActionResult Put(int id, [FromBody] Book book)
    {
        var existingBook = _books.FirstOrDefault(b => b.Id == id);
        if (existingBook == null)
        {
            return NotFound();
        }

        existingBook.Title = book.Title;
        existingBook.Author = book.Author;

        return NoContent();
    }

    [HttpDelete("{id}")]
    public IActionResult Delete(int id)
    {
        var book = _books.FirstOrDefault(b => b.Id == id);
        if (book == null)
        {
            return NotFound();
        }

        _books.Remove(book);
        return NoContent();
    }
}
