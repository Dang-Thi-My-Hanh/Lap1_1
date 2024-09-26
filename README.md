# Lap1_1

<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quản Lý Hiệu Thuốc</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            margin: 0;
            height: 100vh;
            background-image: url('https://media.istockphoto.com/id/1248493102/vi/anh/kh%C3%B4ng-c%C3%B3-g%C3%AC-ngo%C3%A0i-nh%E1%BB%AFng-th%C6%B0%C6%A1ng-hi%E1%BB%87u-t%E1%BB%91t-nh%E1%BA%A5t-cho-kh%C3%A1ch-h%C3%A0ng-c%E1%BB%A7a-h%E1%BB%8D.jpg?s=612x612&w=0&k=20&c=va7lk8j3pDr1DKYRp2CUJP_hsNMU_Fn2vy3t60kMaOM=');
            background-size: cover;
            background-position: center;
            color: rgb(35, 36, 38);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: Arial, sans-serif;
        }
        h1 {
            text-align: center;
            color: #0a0a0bb8;
            margin-bottom: 20px;
            text-shadow: 
                1px 1px 0 white,  /* Viền trắng bên phải và bên dưới */
                -1px -1px 0 white, /* Viền trắng bên trái và phía trên */
                1px -1px 0 white,  /* Viền trắng bên trái và phía trên */
                -1px 1px 0 white;  /* Viền trắng bên phải và bên dưới */
        }
        .container {
            min-width: 900px;
            max-width: 2000px;
            min-height: 700px;
            max-height: 1200px;
            margin: 0 auto;
            background: rgba(114, 187, 223, 0.712);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .form-control {
            margin-bottom: 20px;
        }
        label {
            font-weight: bold;
            display: block;
            margin-bottom: 5px;
        }
        input[type="text"], input[type="number"], input[type="search"] {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            background-color: #f96659;
            color: white;
            border: none;
            padding: 10px 15px;
            margin-right: 10px;
            margin-bottom: 10px;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #f96659;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 12px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        td button {
            background-color: #007bff;
            margin-right: 5px;
        }
        td button:hover {
            background-color: #0056b3;
        }
        td button.delete {
            background-color: #dc3545;
        }
        td button.delete:hover {
            background-color: #c82333;
        }
        .search-control {
            margin-bottom: 20px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Quản Lý Hiệu Thuốc</h1>
        <div>
            <input id="bookId" type="hidden">
            <div class="form-control">
                <label for="bookTitle">Tên thuốc:</label>
                <input id="bookTitle" type="text" placeholder="Nhập tên thuốc">
            </div>
            <div class="form-control">
                <label for="bookAuthor">Công dụng:</label>
                <input id="bookAuthor" type="text" placeholder="Nhập công dụng">
            </div>
            <div class="form-control">
                <label for="bookYear">Hạn sử dụng:</label>
                <input id="bookYear" type="date" placeholder="Nhập hạn sử dụng">
            </div>
            <button onclick="addOrUpdateBook()">Thêm/Sửa thuốc</button>
            <button onclick="resetForm()">Làm mới</button>
        </div>

        <!-- Tìm kiếm thuốc -->
        <div class="search-control">
            <label for="search">Tìm kiếm thuốc:</label>
            <input id="search" type="search" placeholder="Tìm kiếm theo tên thuốc hoặc công dụng" onkeyup="searchBooks()">
        </div>

        <!-- Danh sách thuốc -->
        <table id="bookTable">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Tên thuốc</th>
                    <th>Công dụng</th>
                    <th>Hạn sử dụng</th>
                    <th> </th>
                </tr>
            </thead>
            <tbody>
                <!-- Thuốc sẽ được hiển thị ở đây -->
            </tbody>
        </table>
    </div>

    <script>
        let books = [];
        let editingBookId = null;

        // Hàm thêm hoặc cập nhật thuốc
        function addOrUpdateBook() {
            const bookTitle = document.getElementById('bookTitle').value;
            const bookAuthor = document.getElementById('bookAuthor').value;
            const bookYear = document.getElementById('bookYear').value;

            if (!bookTitle || !bookAuthor || !bookYear) {
                alert("Vui lòng điền đầy đủ thông tin.");
                return;
            }

            if (editingBookId === null) {
                // Thêm sách mới
                const newBook = {
                    id: books.length + 1,
                    title: bookTitle,
                    author: bookAuthor,
                    year: bookYear
                };
                books.push(newBook);
            } else {
                // Cập nhật sách
                const book = books.find(book => book.id === editingBookId);
                book.title = bookTitle;
                book.author = bookAuthor;
                book.year = bookYear;
            }

            resetForm();
            displayBooks();
        }

        // Hàm hiển thị sách
        function displayBooks() {
            const tbody = document.getElementById('bookTable').querySelector('tbody');
            tbody.innerHTML = '';

            books.forEach(book => {
                const row = `
                    <tr>
                        <td>${book.id}</td>
                        <td>${book.title}</td>
                        <td>${book.author}</td>
                        <td>${book.year}</td>
                        <td>
                            <button onclick="editBook(${book.id})">Sửa</button>
                            <button class="delete" onclick="deleteBook(${book.id})">Xóa</button>
                        </td>
                    </tr>
                `;
                tbody.insertAdjacentHTML('beforeend', row);
            });
        }

        // Hàm xóa sách
        function deleteBook(id) {
            books = books.filter(book => book.id !== id);
            displayBooks();
        }

        // Hàm chỉnh sửa sách
        function editBook(id) {
            const book = books.find(book => book.id === id);
            document.getElementById('bookTitle').value = book.title;
            document.getElementById('bookAuthor').value = book.author;
            document.getElementById('bookYear').value = book.year;
            editingBookId = id;
        }

        // Hàm tìm kiếm sách
        function searchBooks() {
            const searchValue = document.getElementById('search').value.toLowerCase();
            const filteredBooks = books.filter(book => 
                book.title.toLowerCase().includes(searchValue) || 
                book.author.toLowerCase().includes(searchValue)
            );
            const tbody = document.getElementById('bookTable').querySelector('tbody');
            tbody.innerHTML = '';

            filteredBooks.forEach(book => {
                const row = `
                    <tr>
                        <td>${book.id}</td>
                        <td>${book.title}</td>
                        <td>${book.author}</td>
                        <td>${book.year}</td>
                        <td>
                            <button onclick="editBook(${book.id})">Sửa</button>
                            <button class="delete" onclick="deleteBook(${book.id})">Xóa</button>
                        </td>
                    </tr>
                `;
                tbody.insertAdjacentHTML('beforeend', row);
            });
        }

        // Hàm reset form
        function resetForm() {
            document.getElementById('bookTitle').value = '';
            document.getElementById('bookAuthor').value = '';
            document.getElementById('bookYear').value = '';
            editingBookId = null;
        }
    </script>

</body>
</html>

