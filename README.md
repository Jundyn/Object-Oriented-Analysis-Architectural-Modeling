```markdown
# Library Management System - Object-Oriented Analysis

A simplified **Library Management System** designed using **Object-Oriented Analysis (OOA)** and architectural planning. The system models real-world entities like books and members, manages borrowing transactions, and supports administrative functions.  

---

## **1. Requirement Analysis**

### **Actors**
- **Librarian:** Manages books, issues and returns, oversees members.  
- **Member:** Borrows and returns books, searches for books.  

### **Key Use Cases**
| Use Case                | Description |
|-------------------------|-------------|
| Search Book             | Member searches books by title, author, or ISBN. |
| Issue Book              | Librarian issues a book to a member. |
| Return Book             | Member returns a borrowed book. |
| Add/Remove Book         | Librarian adds/removes books in the system. |
| Register Member         | Librarian adds new members to the system. |
| View Borrowed Books     | Member or Librarian views borrowed books. |
| Notify Overdue          | System alerts members when books are overdue. |

---

## **2. System Architecture**

### **Components**
1. **UI Layer:** Handles interaction with Members and Librarians.  
2. **Business Logic Layer:** Manages borrowing, returning, and book availability.  
3. **Data Access Layer:** Stores Books, Members, and Transactions.  
4. **Notification System:** Sends alerts for overdue books.  

### **High-Level Architecture Diagram**

```

+------------------+
|      UI Layer    |
|  (Member/Librarian)|
+--------+---------+
|
v
+------------------+
| Business Logic   |
| (LibrarySystem)  |
+--------+---------+
|
v
+------------------+         +------------------+
| Data Access Layer| <-----> | Notification Sys |
| (Books/Members/  |         | (Overdue Alerts) |
|  Transactions)   |         +------------------+
+------------------+

```

---

## **3. Object-Oriented Analysis (OOA)**

### **Classes**

| Class                 | Attributes                          | Methods |
|-----------------------|------------------------------------|---------|
| **Book**              | bookID, title, author, status       | issue(), return(), checkAvailability() |
| **Member (abstract)** | memberID, name, email               | searchBook(), borrowBook(), returnBook() |
| **Student**           | inherited from Member               | - |
| **Teacher**           | inherited from Member               | - |
| **Librarian**         | librarianID, name                   | addBook(), removeBook(), issueBook(), returnBook() |
| **BorrowTransaction** | transactionID, book, member, dateIssued, dateReturned | markReturned(), isOverdue() |
| **LibrarySystem**     | books[], members[], transactions[]  | addBook(), addMember(), borrowBook(), returnBook(), notifyOverdue() |
| **NotificationService** | subscribers[]                      | register(), notifyOverdue() |

---

### **Class Diagram**

```

```
       +----------------+
       |     Book       |
       +----------------+
       | bookID         |
       | title          |
       | author         |
       | status         |
       +----------------+
       | issue()        |
       | return()       |
       | checkAvailability()|
       +----------------+
```

+-------------------+       +-------------------+
|     Member        |<------| BorrowTransaction  |
+-------------------+       +-------------------+
| memberID          |       | transactionID     |
| name              |       | book              |
| email             |       | member            |
+-------------------+       | dateIssued        |
| searchBook()      |       | dateReturned      |
| borrowBook()      |       +-------------------+
| returnBook()      |       | markReturned()    |
+-------------------+       | isOverdue()       |
^
|
+--------------+  +--------------+
|   Student    |  |   Teacher    |
+--------------+  +--------------+

```

---

### **Use Case Diagram (Borrow Book)**

```

Member ---------> (Search Book)
Member ---------> (Borrow Book)
Librarian ------> (Issue Book)
System ---------> (Notify Overdue)

```

---

### **Sequence Diagram (Borrow Book)**

```

Member      LibrarySystem       Book
|              |              |
| borrowBook() |              |
|------------->|              |
|              | checkAvailability() |
|              |------------------>|
|              |  set status=Issued |
|              |<------------------|
|<-------------| confirmation      |

```

---

### **State Diagram (Book)**

```

Available --> Issued --> Returned
^                       |
|-----------------------|

````

---

## **4. Data, Functional, and Behavioral Modeling**

- **Data Model:** Collections for `Books`, `Members`, and `BorrowTransactions`.  
- **Functional Model:** Add book/member, issue/return book, search, notify overdue.  
- **Behavioral Model:** Book transitions: Available → Issued → Returned; Members borrow and return books.  

---

## **5. Pseudocode / Abstraction to Implementation**

```javascript
class Book {
  constructor(bookID, title, author) {
    this.bookID = bookID;
    this.title = title;
    this.author = author;
    this.status = "Available";
  }
  issue() { this.status = "Issued"; }
  return() { this.status = "Returned"; }
}

class Member {
  constructor(id, name) {
    this.id = id;
    this.name = name;
  }
  borrowBook(book) { if(book.status==="Available") book.issue(); }
  returnBook(book) { book.return(); }
}

class LibrarySystem {
  constructor() {
    this.books = [];
    this.members = [];
  }
  addBook(book) { this.books.push(book); }
  addMember(member) { this.members.push(member); }
  borrowBook(memberID, bookID) {
    const book = this.books.find(b=>b.bookID===bookID);
    const member = this.members.find(m=>m.id===memberID);
    member.borrowBook(book);
  }
}
````

---

## **6. Design Principles Applied**

* **Encapsulation:** Classes hide internal state, exposing clear methods.
* **Inheritance:** `Student` and `Teacher` extend `Member`.
* **Modularity:** Separate modules for Users, Books, Transactions, and Notifications.
* **Maintainability:** Easily extendable for new user types, books, or notifications.
* **Behavioral Modeling:** Book lifecycle and borrowing behavior clearly modeled.

---
