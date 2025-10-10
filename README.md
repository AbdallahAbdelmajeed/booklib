using System;
using System.Collections.Generic;
using System.Linq;

namespace LibraryManagementSystem
{

    interface ILibrary
    {
        void AddBook(Book book);
        void ViewBooks();
        void SearchBook(string keyword);
        void BorrowBook(int id);
        void ReturnBook(int id);
        void DeleteBook(int id);
    }

    abstract class Item
    {
        public int ID { get; set; }
        public string Title { get; set; }
        public bool IsAvailable { get; set; }

        public abstract void DisplayInfo();
    }

    
    class Book : Item
    {
        public string Author { get; set; }
        public int Year { get; set; }

        public override void DisplayInfo()
        {
            Console.WriteLine($"ID: {ID} | Title: {Title} | Author: {Author} | Year: {Year} | Available: {IsAvailable}");
        }
    }

    class Library : ILibrary
    {
        private List<Book> books = new List<Book>();

        public void AddBook(Book book)
        {
            books.Add(book);
            Console.WriteLine(" Book added successfully!");
        }

        public void ViewBooks()
        {
            if (books.Count == 0)
            {
                Console.WriteLine(" No books available.");
                return;
            }
            Console.WriteLine("\n All Books:");
            books.ForEach(b => b.DisplayInfo());
        }

        public void SearchBook(string keyword)
        {
            var results = books.Where(b => b.Title.Contains(keyword, StringComparison.OrdinalIgnoreCase)
                                        || b.ID.ToString() == keyword).ToList();

            if (results.Count == 0)
                Console.WriteLine(" No book found.");
            else
                results.ForEach(b => b.DisplayInfo());
        }

        public void BorrowBook(int id)
        {
            var book = books.FirstOrDefault(b => b.ID == id);
            if (book == null)
            {
                Console.WriteLine(" Book not found.");
                return;
            }
            if (!book.IsAvailable)
            {
                Console.WriteLine(" Book already borrowed.");
                return;
            }

            book.IsAvailable = false;
            Console.WriteLine($"You borrowed '{book.Title}' successfully!");
        }

        public void ReturnBook(int id)
        {
            var book = books.FirstOrDefault(b => b.ID == id);
            if (book == null)
            {
                Console.WriteLine(" Book not found.");
                return;
            }

            book.IsAvailable = true;
            Console.WriteLine($" You returned '{book.Title}' successfully!");
        }

        public void DeleteBook(int id)
        {
            var book = books.FirstOrDefault(b => b.ID == id);
            if (book == null)
            {
                Console.WriteLine(" Book not found.");
                return;
            }

            books.Remove(book);
            Console.WriteLine(" Book deleted successfully!");
        }
    }

    class Program
    {
        static void Main()
        {
            Library library = new Library();
            bool running = true;

            while (running)
            {
                Console.WriteLine("\n=====  Library Management System =====");
                Console.WriteLine("1. Add Book");
                Console.WriteLine("2. View All Books");
                Console.WriteLine("3. Search Book");
                Console.WriteLine("4. Borrow Book");
                Console.WriteLine("5. Return Book");
                Console.WriteLine("6. Delete Book");
                Console.WriteLine("7. Exit");
                Console.Write("Choose an option: ");

                string choice = Console.ReadLine();
                switch (choice)
                {
                    case "1":
                        Console.Write("Enter ID: ");
                        int id = int.Parse(Console.ReadLine());
                        Console.Write("Enter Title: ");
                        string title = Console.ReadLine();
                        Console.Write("Enter Author: ");
                        string author = Console.ReadLine();
                        Console.Write("Enter Year: ");
                        int year = int.Parse(Console.ReadLine());
                        library.AddBook(new Book { ID = id, Title = title, Author = author, Year = year, IsAvailable = true });
                        break;

                    case "2":
                        library.ViewBooks();
                        break;

                    case "3":
                        Console.Write("Enter book title or ID: ");
                        library.SearchBook(Console.ReadLine());
                        break;

                    case "4":
                        Console.Write("Enter Book ID to borrow: ");
                        library.BorrowBook(int.Parse(Console.ReadLine()));
                        break;

                    case "5":
                        Console.Write("Enter Book ID to return: ");
                        library.ReturnBook(int.Parse(Console.ReadLine()));
                        break;

                    case "6":
                        Console.Write("Enter Book ID to delete: ");
                        library.DeleteBook(int.Parse(Console.ReadLine()));
                        break;

                    case "7":
                        running = false;
                        Console.WriteLine(" Exiting safely. Goodbye!");
                        break;

                    default:
                        Console.WriteLine(" Invalid choice. Try again.");
                        break;
                }
            }
        }
    }
}
