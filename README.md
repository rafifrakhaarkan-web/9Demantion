<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kelas 9D - Liquid Glass UI</title>
    <style>
        /* Memaksa browser (terutama popup tanggal/dropdown) menggunakan tema gelap */
        :root {
            color-scheme: dark;
            --glass-base: rgba(15, 52, 96, 0.4);
            --glass-border: rgba(165, 216, 255, 0.15);
            --glass-hover: rgba(15, 52, 96, 0.6);
            --text-primary: rgba(255, 255, 255, 0.95);
            --text-secondary: rgba(165, 216, 255, 0.7);
            --primary: #007AFF;
            --success: #34C759;
            --warning: #FF9500;
            --danger: #FF3B30;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            min-height: 100vh;
            padding: 20px;
            color: var(--text-primary);
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '';
            position: fixed;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(0, 122, 255, 0.08) 0%, transparent 60%);
            animation: pulse 15s ease-in-out infinite;
            pointer-events: none;
            z-index: -1;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1) translate(0, 0); opacity: 0.5; }
            50% { transform: scale(1.1) translate(2%, 2%); opacity: 0.8; }
        }

        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: rgba(0, 0, 0, 0.2); }
        ::-webkit-scrollbar-thumb { background: rgba(165, 216, 255, 0.3); border-radius: 10px; }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        .header-title {
            text-align: center;
            margin-bottom: 25px;
            font-size: 32px;
            font-weight: 800;
            letter-spacing: 1px;
            text-transform: uppercase;
            background: linear-gradient(to right, #ffffff, #a5d8ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 4px 15px rgba(0, 122, 255, 0.3);
        }

        /* Glass Cards */
        .glass-card {
            background: var(--glass-base);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            transition: all 0.3s ease;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.25);
        }

        /* Navigation Grid */
        .nav-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .nav-item {
            aspect-ratio: 16/10;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 8px;
            cursor: pointer;
            padding: 10px;
        }

        .nav-item:hover {
            background: var(--glass-hover);
            transform: translateY(-2px);
        }

        .nav-item.active {
            background: linear-gradient(135deg, rgba(0, 122, 255, 0.2) 0%, rgba(15, 52, 96, 0.4) 100%);
            border-color: rgba(165, 216, 255, 0.4);
            box-shadow: 0 0 20px rgba(0, 122, 255, 0.2);
        }

        .nav-item .icon {
            width: 28px;
            height: 28px;
            fill: var(--text-secondary);
            transition: all 0.3s;
        }

        .nav-item.active .icon {
            fill: #ffffff;
            filter: drop-shadow(0 0 5px rgba(255, 255, 255, 0.4));
        }

        .nav-item .label {
            font-size: 13px;
            font-weight: 600;
            color: var(--text-secondary);
        }

        .nav-item.active .label { color: #ffffff; }

        /* Content Area */
        .content-large {
            min-height: 500px;
            padding: 24px;
        }

        .page {
            display: none;
            animation: fadeIn 0.4s ease-out;
        }

        .page.active { display: block; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .page-title {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 20px;
            border-bottom: 1px solid var(--glass-border);
            padding-bottom: 12px;
            color: #ffffff;
        }

        /* Stats & Forms */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }

        .stat-item {
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            padding: 20px;
            text-align: center;
        }

        .stat-item h3 { font-size: 13px; color: var(--text-secondary); margin-bottom: 8px; }
        .stat-item .number { font-size: 32px; font-weight: 800; color: #ffffff; }

        /* Formulir Tanpa Putih Mencolok */
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; font-size: 13px; color: #a5d8ff; margin-bottom: 6px; }

        input[type="text"], input[type="date"], textarea, select {
            width: 100%;
            padding: 14px;
            background: rgba(0, 0, 0, 0.3); /* Gelap untuk membuang putih */
            border: 1px solid rgba(165, 216, 255, 0.2);
            border-radius: 12px;
            color: #ffffff;
            font-size: 14px;
            transition: all 0.3s;
        }

        input:focus, textarea:focus, select:focus {
            outline: none;
            border-color: var(--primary);
            background: rgba(0, 0, 0, 0.5);
        }

        .btn {
            background: linear-gradient(135deg, var(--primary) 0%, #0056b3 100%);
            border: none;
            color: white;
            padding: 14px 24px;
            border-radius: 12px;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            transition: all 0.2s;
        }

        .btn:hover { transform: translateY(-2px); box-shadow: 0 4px 15px rgba(0, 122, 255, 0.4); }

        /* Tabel Absensi (Lebih Elegan & Dark) */
        .table-container {
            background: rgba(0, 0, 0, 0.15);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            overflow-x: auto;
            margin-top: 15px;
        }

        table { width: 100%; border-collapse: collapse; min-width: 500px; }
        
        th {
            background: rgba(0, 0, 0, 0.4);
            padding: 14px 16px;
            text-align: left;
            font-size: 12px;
            color: #a5d8ff;
            text-transform: uppercase;
            border-bottom: 1px solid var(--glass-border);
        }

        td {
            padding: 12px 16px;
            font-size: 14px;
            border-bottom: 1px solid rgba(165, 216, 255, 0.05);
            color: #e2e8f0;
        }

        tr:hover td { background: rgba(0, 122, 255, 0.1); }

        /* Dropdown Status Absen */
        .status-select {
            padding: 6px 10px;
            border-radius: 8px;
            font-weight: 600;
            border: 1px solid transparent;
            cursor: pointer;
        }
        
        /* Opsi Warna untuk JavaScript Dropdown */
        .status-hadir { background: rgba(52, 199, 89, 0.15); color: var(--success); border-color: rgba(52, 199, 89, 0.3); }
        .status-sakit { background: rgba(255, 149, 0, 0.15); color: var(--warning); border-color: rgba(255, 149, 0, 0.3); }
        .status-izin { background: rgba(0, 122, 255, 0.15); color: #a5d8ff; border-color: rgba(0, 122, 255, 0.3); }
        .status-alpa { background: rgba(255, 59, 48, 0.15); color: var(--danger); border-color: rgba(255, 59, 48, 0.3); }

        /* Announcement / Journal Card */
        .journal-card {
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid var(--glass-border);
            padding: 20px;
            border-radius: 16px;
            margin-bottom: 15px;
            animation: fadeIn 0.4s ease-out;
        }

        @media (max-width: 600px) {
            .nav-grid { grid-template-columns: repeat(2, 1fr); }
            .stats-grid { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="header-title">Portal Kelas 9D</h1>

        <div class="nav-grid">
            <div class="glass-card nav-item active" onclick="showPage('dashboard', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M3 13h8V3H3v10zm0 8h8v-6H3v6zm10 0h8V11h-8v10zm0-18v6h8V3h-8z"/></svg>
                <div class="label">Dashboard</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('absen', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M16 11c1.66 0 2.99-1.34 2.99-3S17.66 5 16 5c-1.66 0-3 1.34-3 3s1.34 3 3 3zm-8 0c1.66 0 2.99-1.34 2.99-3S9.66 5 8 5C6.34 5 5 6.34 5 8s1.34 3 3 3zm0 2c-2.33 0-7 1.17-7 3.5V19h14v-2.5c0-2.33-4.67-3.5-7-3.5zm8 0c-.29 0-.62.02-.97.05 1.16.84 1.97 1.97 1.97 3.45V19h6v-2.5c0-2.33-4.67-3.5-7-3.5z"/></svg>
                <div class="label">Absensi</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('jurnal', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M18 2H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zM6 4h5v8l-2.5-1.5L6 12V4z"/></svg>
                <div class="label">Jurnal</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('kas', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M21 18v1c0 1.1-.9 2-2 2H5c-1.11 0-2-.9-2-2V5c0-1.1.89-2 2-2h14c1.1 0 2 .9 2 2v1h-9c-1.11 0-2 .9-2 2v8c0 1.1.89 2 2 2h9zm-9-2h10V8H12v8zm4-2.5c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5z"/></svg>
                <div class="label">Kas Kelas</div>
            </div>
        </div>

        <div class="glass-card content-large">
            
            <div id="dashboard" class="page active">
                <h2 class="page-title">Ringkasan Hari Ini</h2>
                <div class="stats-grid">
                    <div class="stat-item">
                        <h3>Total Siswa</h3>
                        <div class="number">36</div>
                    </div>
                    <div class="stat-item">
                        <h3>Kehadiran</h3>
                        <div class="number" style="color: var(--success);">100%</div>
                    </div>
                    <div class="stat-item">
                        <h3>Saldo Kas</h3>
                        <div class="number" style="color: #a5d8ff; font-size: 26px; line-height: 38px;">Rp 450k</div>
                    </div>
                </div>
                <div class="journal-card">
                    <h2 style="font-size: 16px; color: #a5d8ff; margin-bottom: 8px;">📌 Pengingat</h2>
                    <p style="color: var(--text-secondary); font-size: 14px; line-height: 1.5;">
                        Aplikasi Kelas 9D siap digunakan. Silakan akses menu di atas untuk mengisi data harian.
                    </p>
                </div>
            </div>

            <div id="absen" class="page">
                <h2 class="page-title">Absensi Kelas 9D</h2>
                <div class="form-group" style="max-width: 250px;">
                    <label>Pilih Tanggal</label>
                    <input type="date" id="tanggal-absen">
                </div>
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Nama Siswa</th>
                                <th style="width: 140px;">Status</th>
                            </tr>
                        </thead>
                        <tbody id="absen-tbody">
                            </tbody>
                    </table>
                </div>
            </div>

            <div id="jurnal" class="page">
                <h2 class="page-title">Jurnal Kelas</h2>
                <form id="form-jurnal" onsubmit="simpanJurnal(event)">
                    <div class="stats-grid" style="grid-template-columns: 1fr 1fr; margin-bottom: 0;">
                        <div class="form-group">
                            <label>Mata Pelajaran</label>
                            <input type="text" id="mapel" placeholder="Cth: Matematika" required>
                        </div>
                        <div class="form-group">
                            <label>Nama Guru</label>
                            <input type="text" id="guru" placeholder="Cth: Bpk. Budi" required>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Ringkasan Materi</label>
                        <textarea id="materi" rows="3" placeholder="Tulis catatan materi di sini..." required></textarea>
                    </div>
                    <button type="submit" class="btn">Simpan Jurnal</button>
                </form>

                <h3 style="margin-top: 30px; margin-bottom: 15px; font-size: 16px; color: #a5d8ff;">Catatan Terakhir</h3>
                <div id="history-jurnal">
                    </div>
            </div>

            <div id="kas" class="page">
                <h2 class="page-title">Keuangan Kelas</h2>
                <div class="stats-grid" style="grid-template-columns: 1fr 1fr;">
                    <div class="stat-item">
                        <h3>Pemasukan</h3>
                        <div class="number" style="color: var(--success); font-size: 24px;">+ Rp 500.000</div>
                    </div>
                    <div class="stat-item">
                        <h3>Pengeluaran</h3>
                        <div class="number" style="color: var(--danger); font-size: 24px;">- Rp 50.000</div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <script>
        // 1. Data 36 Siswa Kelas 9D
        const namaSiswa = [
            "Agung Ramaindra Syahputra", "Ainun Tiyas martiningsih", "Alessia Alzena Riangga",
            "Almaira Faiqha", "Alya Faliha", "Amirah Julveni Maulana", "Andi Aqil",
            "Aurora Felicia Angelie", "Bilqis Melani", "Chaya muja syatira",
            "Fadhil oktavian nabil", "Fahmi Firjatullah", "faiz Alfarizi",
            "farhan umaydillah radjiansyah", "fathir pradipta alfarizi", "fazril nizart",
            "januari", "jihaan kaltsum khairunnisa", "juan edra abiya",
            "Khanza Rista Bhanuwati", "Kristiana Berta", "Marselino Prasetyo wijaya",
            "M. Fadil Putra pratama", "M. Kafka adri Al-Maliq", "M. Luthfi faturrahman",
            "Natalis Fedora Purba", "FAHMI FIRJATULLAH", "Rayhan Qolbu",
            "Runika Alzariva", "Suci Pratiwi", "Syafa Damia Sakhi",
            "Syech Faris maulana", "Tegar Tri Pambudi", "Tersa Usela",
            "Tiara Husna Humairah", "Zhafira Mayarista"
        ];

        // Set tanggal absen default hari ini
        document.getElementById('tanggal-absen').valueAsDate = new Date();

        // Mengisi Tabel Absensi Otomatis
        const tbody = document.getElementById('absen-tbody');
        let htmlAbsen = '';
        namaSiswa.forEach((nama, index) => {
            htmlAbsen += `
                <tr>
                    <td>${index + 1}</td>
                    <td style="text-transform: capitalize;">${nama}</td>
                    <td>
                        <select class="status-select status-hadir" onchange="ubahWarnaSelect(this)">
                            <option value="hadir">Hadir</option>
                            <option value="sakit">Sakit</option>
                            <option value="izin">Izin</option>
                            <option value="alpa">Alpa</option>
                        </select>
                    </td>
                </tr>
            `;
        });
        tbody.innerHTML = htmlAbsen;

        // Mengubah warna dropdown absen ketika diganti
        function ubahWarnaSelect(element) {
            // Hapus semua class status
            element.classList.remove('status-hadir', 'status-sakit', 'status-izin', 'status-alpa');
            // Tambahkan class sesuai nilai yang dipilih
            element.classList.add(`status-${element.value}`);
        }

        // 2. Fungsi Navigasi Tab
        function showPage(pageId, element) {
            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');

            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            element.classList.add('active');
        }

        // 3. Fungsi Simpan Jurnal (History bekerja tanpa reload)
        function simpanJurnal(event) {
            event.preventDefault(); // Mencegah halaman reload
            
            const mapel = document.getElementById('mapel').value;
            const guru = document.getElementById('guru').value;
            const materi = document.getElementById('materi').value;
            
            // Format tanggal saat ini
            const dateStr = new Date().toLocaleDateString('id-ID', {day: 'numeric', month: 'long', year: 'numeric'});

            // Buat elemen history baru
            const historyContainer = document.getElementById('history-jurnal');
            const itemHTML = `
                <div class="journal-card">
                    <div style="font-size: 12px; color: var(--primary); margin-bottom: 6px; font-weight: 600;">
                        🗓️ ${dateStr} • ${guru}
                    </div>
                    <strong style="font-size: 16px; color: #ffffff;">${mapel}</strong>
                    <p style="margin-top: 8px; color: var(--text-secondary); font-size: 14px; line-height: 1.5;">
                        ${materi}
                    </p>
                </div>
            `;
            
            // Masukkan data baru di posisi paling atas
            historyContainer.insertAdjacentHTML('afterbegin', itemHTML);

            // Bersihkan formulir setelah tersimpan
            document.getElementById('form-jurnal').reset();
            alert("Berhasil! Jurnal telah tersimpan di riwayat.");
        }
    </script>
</body>
</html>
