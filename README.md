# Library Management System

A comprehensive database-driven library management system built with SQL for tracking books, members, and loan transactions.

## 📚 Overview

This library management system provides a complete solution for managing library operations including book inventory, member registration, loan tracking, and analytics. The system is designed to handle common library workflows and generate useful reports for library administration.

## 🗃️ Database Schema

The system consists of three main tables:

### Books Table
- `book_id` (INT, PRIMARY KEY) - Unique identifier for each book
- `title` (VARCHAR(100)) - Book title
- `author` (VARCHAR(100)) - Book author
- `genre` (VARCHAR(50)) - Book genre/category
- `available_copies` (INT) - Number of copies available for loan

### Members Table
- `member_id` (INT, PRIMARY KEY, AUTO_INCREMENT) - Unique member identifier
- `name` (VARCHAR(100)) - Member's full name
- `join_date` (DATE) - Date when member joined the library

### Loans Table
- `loan_id` (INT, PRIMARY KEY, AUTO_INCREMENT) - Unique loan identifier
- `book_id` (INT, FOREIGN KEY) - References books table
- `member_id` (INT, FOREIGN KEY) - References members table
- `loan_date` (DATE) - Date when book was borrowed
- `return_date` (DATE) - Date when book was returned (NULL if not returned)

## 🔍 Key Features

### Analytics & Reporting
The system includes several built-in analytical queries:

1. **Member Borrowing Statistics** - Track how many books each member has borrowed
2. **Popular Books Analysis** - Identify most frequently borrowed books
3. **Overdue Books Management** - Monitor books that haven't been returned
4. **Genre Popularity Tracking** - Analyze which book genres are most popular

### Sample Queries Implemented

#### Books Borrowed by Each Member
```sql
SELECT members.name, COUNT(loans.loan_id) AS books_borrowed
FROM members
LEFT JOIN loans ON members.member_id = loans.member_id
GROUP BY members.name;
```

#### Most Borrowed Books
```sql
SELECT books.title, COUNT(loans.loan_id) AS times_borrowed
FROM books
JOIN loans ON books.book_id = loans.book_id
GROUP BY books.title
ORDER BY times_borrowed DESC;
```

#### Overdue Books (Not Returned)
```sql
SELECT books.title, members.name, loans.loan_date
FROM loans
JOIN books ON loans.book_id = books.book_id
JOIN members ON loans.member_id = members.member_id
WHERE loans.return_date IS NULL;
```

#### Genre Popularity
```sql
SELECT genre, COUNT(loans.loan_id) AS total_loans
FROM books
JOIN loans ON books.book_id = loans.book_id
GROUP BY genre
ORDER BY total_loans DESC;
```

## 📊 Sample Data Results

Based on the current dataset:

- **Active Members**: Alice Smith (2 books borrowed), Bob Johnson (1 book borrowed)
- **Popular Books**: The Hobbit, 1984, To Kill a Mockingbird (each borrowed once)
- **Overdue Items**: 1984 borrowed by Alice Smith since March 15, 2024
- **Genre Distribution**: Fantasy, Dystopian, and Classic genres each have 1 loan


### Installation

1. **Create the Database**
   ```sql
   CREATE DATABASE library_db;
   USE library_db;
   ```

2. **Create Tables**
   Execute the table creation scripts in the following order:
   - Create `books` table
   - Create `members` table  
   - Create `loans` table (with foreign key constraints)

3. **Insert Sample Data**
   Populate tables with initial data for testing

4. **Run Analytics Queries**
   Execute the provided analytical queries to generate reports

## 🔧 Usage Examples

### Adding a New Book
```sql
INSERT INTO books (book_id, title, author, genre, available_copies)
VALUES (4, 'Pride and Prejudice', 'Jane Austen', 'Romance', 2);
```

### Registering a New Member
```sql
INSERT INTO members (name, join_date)
VALUES ('Carol Davis', '2024-06-01');
```

### Recording a Book Loan
```sql
INSERT INTO loans (book_id, member_id, loan_date)
VALUES (4, 3, '2024-06-03');
```

### Processing a Book Return
```sql
UPDATE loans 
SET return_date = '2024-06-10'
WHERE loan_id = 1;
```




---

