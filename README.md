## 🚀 Deployment <a name = "deployment"></a>

Get Postgres
```
sudo apt install postgresql postgresql-contribp
```
Start Postgres
```
sudo service postgresql start
```
Connect to postgres service and open the psql shell
```
sudo -u postgres psql
```
Initialize Database by creating a new database called `bookstore`
```
CREATE DATABASE bookstore;
```
```
\c bookstore
```
```
CREATE TABLE books (
  isbn    char(14) NOT NULL,
  title   varchar(255) NOT NULL,
  author  varchar(255) NOT NULL,
  price   decimal(5,2) NOT NULL
);

INSERT INTO books (isbn, title, author, price) VALUES
('978-1503261969', 'Emma', 'Jayne Austen', 9.44),
('978-1505255607', 'The Time Machine', 'H. G. Wells', 5.99),
('978-1503379640', 'The Prince', 'Niccolò Machiavelli', 6.99);

ALTER TABLE books ADD PRIMARY KEY (isbn);
```

Create User:
```
create user myuser with encrypted password 'mypass';
```

Grant privileges to `myuser`:
```
ALTER USER myuser WITH SUPERUSER;
```

```
go run main.go
```

## Example Requests
```
$ curl -i localhost:3000/books
HTTP/1.1 200 OK
Content-Length: 205
Content-Type: text/plain; charset=utf-8

978-1503261969, Emma, Jayne Austen, £9.44
978-1505255607, The Time Machine, H. G. Wells, £5.99
978-1503379640, The Prince, Niccolò Machiavelli, £6.99
```
```
$ curl -i "localhost:3000/books/show?isbn=978-1505255607"
HTTP/1.1 200 OK
Content-Length: 54
Content-Type: text/plain; charset=utf-8

978-1505255607, The Time Machine, H. G. Wells, £5.99
```
```
$ curl -i -X POST -d "isbn=978-1470184841&title=Metamorphosis&author=Franz Kafka&price=5.90" localhost:3000/books/create
HTTP/1.1 200 OK
Content-Length: 58
Content-Type: text/plain; charset=utf-8

Book 978-1470184841 created successfully (1 row affected)
```