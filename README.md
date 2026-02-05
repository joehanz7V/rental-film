<!DOCTYPE html>
<html>
<head>
  <title>Rental Film</title>
</head>
<body>
  <h1>Rental Film</h1>
  <form>
    <label>Judul Film:</label>
    <input type="text" id="judul">
    <br>
    <label>Genre:</label>
    <select id="genre">
      <option>Komedi</option>
      <option>Action</option>
      <option>Drama</option>
    </select>
    <br>
    <label>Harga:</label>
    <input type="number" id="harga">
    <br>

  </form>
  
    <button onclick="tambahFilm(event)">Tambah Film</button>
    
  <ul id="daftar-film"></ul>
  
  <button onclick="sortFilm('judul')">Sort by Judul</button>
  
<button onclick="sortFilm('harga')">Sort by Harga</button>

<input type="text" id="cari" placeholder="Cari film...">

<button onclick="cariFilm()">Cari</button>

<button onclick="exportJSON()">Export ke JSON</button>

<button onclick="simpanData()">Simpan Data</button>

<input type="file" id="gambar">

<select id="rating">
  <option value="1">1/5</option>
  <option value="2">2/5</option>
  <option value="3">3/5</option>
  <option value="4">4/5</option>
  <option value="5">5/5</option>
</select>

<select id="filter-genre" onchange="filterFilm()">
  <option value="all">Semua</option>
  <option value="Komedi">Komedi</option>
  <option value="Action">Action</option>
  <!-- Tambahkan genre lain -->
</select>

  <script>
    function tambahFilm() {
      let judul = document.getElementById('judul').value;
      let genre = document.getElementById('genre').value;
      let harga = document.getElementById('harga').value;
      alert('Film ' + judul + ' berhasil ditambahkan!');
    }

let filmList = [];
let editIndex = null;

function tambahFilm(event) {
  event.preventDefault();
  let judul = document.getElementById('judul').value;
  let genre = document.getElementById('genre').value;
  let harga = document.getElementById('harga').value;
  
  if (editIndex === null) {
    filmList.push({ judul, genre, harga });
  } else {
    filmList[editIndex] = { judul, genre, harga };
    editIndex = null;
  }
  tampilkanFilm();
  document.getElementById('judul').value = '';
  document.getElementById('genre').value = 'Komedi';
  document.getElementById('harga').value = '';
}

function tampilkanFilm() {
  let daftarFilm = document.getElementById('daftar-film');
  daftarFilm.innerHTML = '';
  filmList.forEach((film, index) => {
    daftarFilm.innerHTML += `
      <li>
        ${film.judul} (${film.genre}) - Rp ${film.harga}
        <button onclick="editFilm(${index})">Edit</button>
        <button onclick="hapusFilm(${index})">Hapus</button>
      </li>
    `;
  });
}

function hapusFilm(index) {
  filmList.splice(index, 1);
  tampilkanFilm();
}

function editFilm(index) {
  let film = filmList[index];
  document.getElementById('judul').value = film.judul;
  document.getElementById('genre').value = film.genre;
  document.getElementById('harga').value = film.harga;
  editIndex = index;
}
function sortFilm(by) {
  if (by === 'judul') {
    filmList.sort((a, b) => a.judul.localeCompare(b.judul));
  } else if (by === 'harga') {
    filmList.sort((a, b) => a.harga - b.harga);
  }
  tampilkanFilm();
}
function cariFilm() {
  let keyword = document.getElementById('cari').value.toLowerCase();
  let hasilCari = filmList.filter(film => 
    film.judul.toLowerCase().includes(keyword) || 
    film.genre.toLowerCase().includes(keyword)
  );
  let daftarFilm = document.getElementById('daftar-film');
  daftarFilm.innerHTML = '';
  hasilCari.forEach((film, index) => {
    daftarFilm.innerHTML += `
      <li>
        ${film.judul} (${film.genre}) - Rp ${film.harga}
        <button onclick="editFilm(${filmList.indexOf(film)})">Edit</button>
        <button onclick="hapusFilm(${filmList.indexOf(film)})">Hapus</button>
      </li>
    `;
  });
}
function tambahFilm(event) {
  event.preventDefault();
  let judul = document.getElementById('judul').value;
  let genre = document.getElementById('genre').value;
  let harga = document.getElementById('harga').value;
  
  if (judul === '' || harga === '') {
    alert('Judul dan Harga harus diisi!');
    return;
  }
  
  if (isNaN(harga) || harga <= 0) {
    alert('Harga harus angka positif!');
    return;
  }
  
  if (editIndex === null) {
    filmList.push({ judul, genre, harga });
  } else {
    filmList[editIndex] = { judul, genre, harga };
    editIndex = null;
  }
  tampilkanFilm();
  document.getElementById('judul').value = '';
  document.getElementById('genre').value = 'Komedi';
  document.getElementById('harga').value = '';
}
function exportJSON() {
  let dataJSON = JSON.stringify(filmList, null, 2);
  let blob = new Blob([dataJSON], {type: 'application/json'});
  let link = document.createElement('a');
  link.href = URL.createObjectURL(blob);
  link.download = 'film.json';
  link.click();
}
function simpanData() {
  localStorage.setItem('filmList', JSON.stringify(filmList));
  alert('Data disimpan!');
}

function loadData() {
  let data = localStorage.getItem('filmList');
  if (data) {
    filmList = JSON.parse(data);
    tampilkanFilm();
  }
}

// Panggil loadData() saat halaman dimuat
window.onload = function() {
  loadData();
};

let gambar = document.getElementById('gambar').files[0];
if (gambar) {
  let reader = new FileReader();
  reader.onload = function(e) {
    filmList.push({
      judul,
      genre,
      harga,
      gambar: e.target.result
    });
    tampilkanFilm();
  };
  reader.readAsDataURL(gambar);
}

function filterFilm() {
  let genre = document.getElementById('filter-genre').value;
  let filtered = filmList;
  if (genre !== 'all') {
    filtered = filmList.filter(film => film.genre === genre);
  }
  // Update tampilan film dengan data filtered
}

let rating = document.getElementById('rating').value;
filmList.push({ judul, genre, harga, rating });

  </script>

</body>
</html>
