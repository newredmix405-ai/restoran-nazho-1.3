from flask import Flask, request, render_template_string

app = Flask(__name__)

# Template halaman utama
index_html = """
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Restoran Nazho</title>
</head>
<body>
  <h1>Selamat Datang di Restoran Nazho</h1>
  <nav>
    <a href="/">Beranda</a> |
    <a href="/menu">Menu</a> |
    <a href="/kontak">Kontak</a>
  </nav>
  <p>Tempat makan enak dengan harga bersahabat!</p>
</body>
</html>
"""

# Template halaman menu
menu_html = """
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Menu Restoran Nazho</title>
</head>
<body>
  <h1>Daftar Menu</h1>
  <ul>
    {% for item in makanan %}
      <li>{{ item.nama }} - Rp {{ item.harga }}</li>
    {% endfor %}
  </ul>
  <a href="/">Kembali</a>
</body>
</html>
"""

# Template halaman kontak
kontak_html = """
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Kontak Restoran Nazho</title>
</head>
<body>
  <h1>Hubungi Kami</h1>
  <form method="POST">
    <label>Nama:</label><br>
    <input type="text" name="nama" required><br><br>
    <label>Pesan:</label><br>
    <textarea name="pesan" required></textarea><br><br>
    <button type="submit">Kirim</button>
  </form>
  <a href="/">Kembali</a>
</body>
</html>
"""

# Route halaman utama
@app.route('/')
def home():
    return render_template_string(index_html)

# Route halaman menu
@app.route('/menu')
def menu():
    makanan = [
        {"nama": "Nasi Goreng Spesial", "harga": 25000},
        {"nama": "Ayam Bakar Madu", "harga": 30000},
        {"nama": "Sate Ayam", "harga": 20000},
        {"nama": "Es Teh Manis", "harga": 5000},
        {"nama": "Jus Alpukat", "harga": 15000},
    ]
    return render_template_string(menu_html, makanan=makanan)

# Route halaman kontak
@app.route('/kontak', methods=["GET", "POST"])
def kontak():
    if request.method == "POST":
        nama = request.form['nama']
        pesan = request.form['pesan']
        return f"<h2>Terima kasih {nama}, pesanmu sudah kami terima!</h2><a href='/'>Kembali</a>"
    return render_template_string(kontak_html)

if __name__ == "__main__":
    app.run(debug=True)
