    public class BooksController : ControllerBase
    {
        private readonly List<Book> _books = new List<Book>
        {
            new Book { Id = 1, Title = "Sample Book 1", Author = "John Doe" },
            new Book { Id = 2, Title = "Sample Book 2", Author = "Jane Smith" }
        };

        // GET: api/books
        [HttpGet]
        public ActionResult<IEnumerable<Book>> Get()
        {
            return Ok(_books);
        }

        // GET: api/books/1
        [HttpGet("{id}")]
        public ActionResult<Book> Get(int id)
        {
            var book = _books.Find(b => b.Id == id);
            if (book == null)
                return NotFound();

            return Ok(book);
        }

        // POST: api/books
        [HttpPost]
        public ActionResult<Book> Post([FromBody] Book book)
        {
            book.Id = _books.Count + 1;
            _books.Add(book);
            return CreatedAtAction(nameof(Get), new { id = book.Id }, book);
        }

        // PUT: api/books/1
        [HttpPut("{id}")]
        public ActionResult<Book> Put(int id, [FromBody] Book book)
        {
            var existingBook = _books.Find(b => b.Id == id);
            if (existingBook == null)
                return NotFound();

            existingBook.Title = book.Title;
            existingBook.Author = book.Author;

            return NoContent();
        }

        // DELETE: api/books/1
        [HttpDelete("{id}")]
        public ActionResult Delete(int id)
        {
            var bookToRemove = _books.Find(b => b.Id == id);
            if (bookToRemove == null)
                return NotFound();

            _books.Remove(bookToRemove);
            return NoContent();
        }
    }
}
