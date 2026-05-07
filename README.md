<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kelas 9D - Liquid Glass UI</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        :root {
            --glass-white: rgba(255, 255, 255, 0.1);
            --glass-border: rgba(255, 255, 255, 0.2);
            --glass-strong: rgba(255, 255, 255, 0.15);
            --text-primary: rgba(255, 255, 255, 0.95);
            --text-secondary: rgba(255, 255, 255, 0.7);
            --primary: #007AFF;
            --success: #34C759;
            --warning: #FF9500;
            --danger: #FF3B30;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', sans-serif;
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
            background: radial-gradient(circle, rgba(0, 122, 255, 0.1) 0%, transparent 70%);
            animation: pulse 15s ease-in-out infinite;
            pointer-events: none;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1) translate(0, 0); opacity: 0.5; }
            50% { transform: scale(1.1) translate(5%, 5%); opacity: 0.8; }
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        /* Navigation Grid */
        .glass-card {
            background: var(--glass-white);
            backdrop-filter: saturate(180%) blur(20px);
            -webkit-backdrop-filter: saturate(180%) blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .nav-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            margin-bottom: 12px;
        }

        .nav-item {
            aspect-ratio: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 8px;
            cursor: pointer;
            padding: 20px;
            text-align: center;
        }

        .nav-item.active {
            background: var(--glass-strong);
            border-color: rgba(255, 255, 255, 0.4);
        }

        .nav-item .label {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-secondary);
        }

        .nav-item.active .label {
            color: var(--text-primary);
        }

        /* Stats Grid */
        .content-large {
            min-height: 400px;
            padding: 28px;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 16px;
            margin-bottom: 24px;
        }

        .stat-item {
            background: var(--glass-white);
            backdrop-filter: saturate(180%) blur(20px);
            -webkit-backdrop-filter: saturate(180%) blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            padding: 20px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .stat-item h3 {
            font-size: 13px;
            font-weight: 600;
            color: var(--text-secondary);
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .stat-item .number {
            font-size: 36px;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .stat-item .label {
            font-size: 12px;
            color: var(--text-secondary);
        }

        /* Table (Absen & Kas) */
        .table-container {
            background: var(--glass-white);
            backdrop-filter: saturate(180%) blur(20px);
            -webkit-backdrop-filter: saturate(180%) blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            overflow-x: auto; /* Untuk responsif di HP */
            margin-top: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th {
            background: rgba(255, 255, 255, 0.05); /* Tidak putih mencolok */
            padding: 16px 20px;
            text-align: left;
            font-size: 13px;
            font-weight: 600;
            color: var(--text-secondary);
            text-transform: uppercase;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        td {
            padding: 16px 20px;
            font-size: 15px;
            font-weight: 500;
            color: var(--text-primary);
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }

        tr:last-child td { border-bottom: none; }
        tr:hover { background: rgba(255, 255, 255, 0.03); }

        /* Pages */
        .page {
            display: none;
            animation: fadeIn 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .page.active { display: block; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .page-title {
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 24px;
        }

        /* Forms & Inputs */
        .form-group { margin-bottom: 20px; }
        .form-group label {
            display: block;
            font-size: 14px;
            font-weight: 600;
            color: var(--text-secondary);
            margin-bottom: 8px;
        }
        
        input[type="text"], input[type="date"], textarea, input[type="number"] {
            width: 100%;
            padding: 14px 18px;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 14px;
            color: var(--text-primary);
            font-family: inherit;
            font-size: 15px;
        }

        textarea { resize: vertical; min-height: 100px; }
        input:focus, textarea:focus {
            outline: none;
            background: rgba(255, 255, 255, 0.12);
            border-color: rgba(255, 255, 255, 0.3);
        }

        /* Buttons */
        .btn {
            background: rgba(255, 255, 255, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: var(--text-primary);
            padding: 14px 28px;
            border-radius: 14px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 12px;
        }
        .btn:hover { background: rgba(255, 255, 255, 0.2); transform: translateY(-2px); }

        /* Journal Items */
        .journal-list { margin-top: 24px; }
        .journal-item {
            background: var(--glass-white);
            border: 1px solid var(--glass-border);
            padding: 20px;
            border-radius: 16px;
            margin-bottom: 12px;
        }
        .journal-item h3 { font-size: 17px; margin-bottom: 10px; color: var(--primary); }
        .journal-item p { font-size: 14px; color: var(--text-primary); line-height: 1.5; margin-bottom: 8px; }
        .journal-item .date { color: var(--text-secondary); font-size: 12px; }

        /* Kas */
        .kas-input {
            width: 120px !important;
            padding: 8px 12px !important;
        }

        /* Radio Absen Theme */
        .radio-group {
            display: flex;
            gap: 15px;
        }
        .radio-label {
            display: flex;
            align-items: center;
            gap: 5px;
            cursor: pointer;
            font-size: 14px;
            color: var(--text-secondary);
        }
        .radio-label input[type="radio"] {
            accent-color: var(--primary);
            width: 18px;
            height: 18px;
        }
        .radio-label:hover { color: var(--text-primary); }

        /* Responsive */
        @media (max-width: 1024px) { .nav-grid { grid-template-columns: repeat(2, 1fr); } }
        @media (max-width: 640px) {
            .stats-grid { grid-template-columns: 1fr 1fr; }
            .radio-group { flex-direction: column; gap: 8px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="nav-grid">
            <div class="glass-card nav-item active" onclick="showPage('dashboard')">
                <div class="label">Dashboard</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('absen')">
                <div class="label">Absensi</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('jurnal')">
                <div class="label">Jurnal</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('kas')">
                <div class="label">Uang Kas</div>
            </div>
        </div>

        <div class="glass-card content-large">
            
            <div id="dashboard" class="page active">
                <h2 class="page-title">Dashboard 9D</h2>
                <div class="stats-grid">
                    <div class="stat-item">
                        <h3>Total Siswa</h3>
                        <div class="number">36</div>
                        <div class="label">Terdaftar</div>
                    </div>
                    <div class="stat-item">
                        <h3>Hadir</h3>
                        <div class="number" id="stat-hadir" style="color: var(--success);">0</div>
                        <div class="label">Siswa Hari Ini</div>
                    </div>
                    <div class="stat-item">
                        <h3>Sakit / Izin</h3>
                        <div class="number" id="stat-sakitizin" style="color: var(--warning);">0</div>
                        <div class="label">Siswa Hari Ini</div>
                    </div>
                    <div class="stat-item">
                        <h3>Alpa</h3>
                        <div class="number" id="stat-alpa" style="color: var(--danger);">0</div>
                        <div class="label">Siswa Hari Ini</div>
                    </div>
                    <div class="stat-item" style="grid-column: span 2;">
                        <h3>Total Kas Terkumpul</h3>
                        <div class="number" id="stat-kas" style="font-size: 28px; color: var(--primary);">Rp 0</div>
                        <div class="label">Bulan Ini</div>
                    </div>
                </div>
            </div>

            <div id="absen" class="page">
                <h2 class="page-title">Absensi Siswa</h2>
                <p style="color: var(--text-secondary); margin-bottom: 10px;">Pilih status kehadiran siswa hari ini.</p>
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Nama Siswa</th>
                                <th>Status Kehadiran</th>
                            </tr>
                        </thead>
                        <tbody id="absen-tbody">
                            </tbody>
                    </table>
                </div>
                <button class="btn" onclick="simpanAbsen()" style="width: 100%; margin-top: 20px;">Simpan Absensi</button>
            </div>

            <div id="jurnal" class="page">
                <h2 class="page-title">Jurnal Kelas</h2>
                <div class="form-group">
                    <label>Mata Pelajaran</label>
                    <input type="text" id="jurnal-mapel" placeholder="Contoh: Matematika">
                </div>
                <div class="form-group">
                    <label>Kegiatan / Catatan</label>
                    <textarea id="jurnal-kegiatan" placeholder="Tuliskan materi atau kegiatan hari ini..."></textarea>
                </div>
                <button class="btn" onclick="simpanJurnal()">Tambah ke History Jurnal</button>

                <div class="journal-list" id="history-jurnal">
                    </div>
            </div>

            <div id="kas" class="page">
                <h2 class="page-title">Pembayaran Uang Kas</h2>
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Nama Siswa</th>
                                <th>Jumlah Bayar (Rp)</th>
                            </tr>
                        </thead>
                        <tbody id="kas-tbody">
                            </tbody>
                    </table>
                </div>
                <button class="btn" onclick="simpanKas()" style="width: 100%; margin-top: 20px;">Update Total Kas</button>
            </div>

        </div>
    </div>

    <script>
        // Data Nama Siswa
        const daftarSiswa = [
            "Agung Ramaindra Syahputra", "Ainun Tiyas martiningsih", "Alessia Alzena Riangga",
            "Almaira Faiqha", "Alya Faliha", "Amirah Julveni Maulana", "Andi Aqil",
            "Aurora Felicia Angelie", "Bilqis Melani", "Chaya muja syatira", "Fadhil oktavian nabil",
            "fahmi firjatullah", "faiz Alfarizi", "farhan umaydillah radjiansyah", "fathir pradipta alfarizi",
            "fazril nizart", "januari", "jihaan kaltsum khairunnisa", "juan edra abiya",
            "Khanza Rista Bhanuwati", "Kristiana Berta", "Marselino Prasetyo wijaya", "M. Fadil Putra pratama",
            "M. Kafka adri Al-Maliq", "M. Luthfi faturrahman", "Natalis Fedora Purba", "Rafif Rakha Arkana",
            "Rayhan Qolbu", "Runika Alzariva", "Suci Pratiwi", "Syafa Damia Sakhi", "Syech Faris maulana",
            "Tegar Tri Pambudi", "Tersa Usela", "Tiara Husna Humairah", "Zhafira Mayarista"
        ];

        // 1. Fungsi Navigasi Tab
        function showPage(pageId) {
            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
            
            document.getElementById(pageId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        // 2. Render Tabel Absen & Kas saat halaman dimuat
        window.onload = function() {
            renderTabelSiswa();
            muatHistoryJurnal();
            muatDataKas();
            hitungStatistikAbsen(); // Jika ada data absen di memori
        };

        function renderTabelSiswa() {
            const absenTbody = document.getElementById('absen-tbody');
            const kasTbody = document.getElementById('kas-tbody');
            
            let absenHTML = '';
            let kasHTML = '';

            daftarSiswa.forEach((nama, index) => {
                const no = index + 1;
                // HTML Absen
                absenHTML += `
                    <tr>
                        <td>${no}</td>
                        <td>${nama}</td>
                        <td>
                            <div class="radio-group">
                                <label class="radio-label"><input type="radio" name="absen_${no}" value="Hadir" checked> Hadir</label>
                                <label class="radio-label"><input type="radio" name="absen_${no}" value="Sakit"> Sakit</label>
                                <label class="radio-label"><input type="radio" name="absen_${no}" value="Izin"> Izin</label>
                                <label class="radio-label"><input type="radio" name="absen_${no}" value="Alpa"> Alpa</label>
                            </div>
                        </td>
                    </tr>
                `;

                // HTML Kas
                kasHTML += `
                    <tr>
                        <td>${no}</td>
                        <td>${nama}</td>
                        <td>
                            <input type="number" id="kas_${no}" class="kas-input" placeholder="0" min="0">
                        </td>
                    </tr>
                `;
            });

            absenTbody.innerHTML = absenHTML;
            kasTbody.innerHTML = kasHTML;
        }

        // 3. Logika Absensi (Hitung ke Dashboard)
        function simpanAbsen() {
            let hadir = 0, sakit = 0, izin = 0, alpa = 0;

            for (let i = 1; i <= daftarSiswa.length; i++) {
                let status = document.querySelector(`input[name="absen_${i}"]:checked`).value;
                if (status === "Hadir") hadir++;
                else if (status === "Sakit") sakit++;
                else if (status === "Izin") izin++;
                else if (status === "Alpa") alpa++;
            }

            document.getElementById('stat-hadir').innerText = hadir;
            document.getElementById('stat-sakitizin').innerText = sakit + izin;
            document.getElementById('stat-alpa').innerText = alpa;
            
            alert('Data absensi hari ini berhasil diperbarui ke Dashboard!');
        }

        // 4. Logika Jurnal History (Simpan ke LocalStorage agar tidak hilang)
        function simpanJurnal() {
            const mapel = document.getElementById('jurnal-mapel').value;
            const kegiatan = document.getElementById('jurnal-kegiatan').value;
            const tanggal = new Date().toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });

            if(mapel === '' || kegiatan === '') {
                alert('Mata pelajaran dan kegiatan harus diisi!');
                return;
            }

            const jurnalBaru = { mapel, kegiatan, tanggal };
            
            // Ambil data lama
            let listJurnal = JSON.parse(localStorage.getItem('jurnal_9d')) || [];
            // Masukkan data baru di posisi atas
            listJurnal.unshift(jurnalBaru);
            // Simpan kembali
            localStorage.setItem('jurnal_9d', JSON.stringify(listJurnal));

            // Bersihkan form
            document.getElementById('jurnal-mapel').value = '';
            document.getElementById('jurnal-kegiatan').value = '';

            // Render ulang
            muatHistoryJurnal();
        }

        function muatHistoryJurnal() {
            const container = document.getElementById('history-jurnal');
            let listJurnal = JSON.parse(localStorage.getItem('jurnal_9d')) || [];
            
            if (listJurnal.length === 0) {
                container.innerHTML = '<p style="color: var(--text-secondary);">Belum ada history jurnal. Tambahkan di atas.</p>';
                return;
            }

            let html = '';
            listJurnal.forEach(item => {
                html += `
                    <div class="journal-item">
                        <h3>${item.mapel}</h3>
                        <p>${item.kegiatan}</p>
                        <div class="date">${item.tanggal}</div>
                    </div>
                `;
            });
            container.innerHTML = html;
        }

        // 5. Logika Uang Kas
        function simpanKas() {
            let totalKas = 0;
            let dataKasSiswa = [];

            for (let i = 1; i <= daftarSiswa.length; i++) {
                let nominal = parseInt(document.getElementById(`kas_${i}`).value) || 0;
                totalKas += nominal;
                dataKasSiswa.push(nominal);
            }

            // Simpan total ke Dashboard
            const formatRupiah = new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(totalKas);
            document.getElementById('stat-kas').innerText = formatRupiah;

            // Simpan data inputan agar tidak hilang jika di-refresh (opsional)
            localStorage.setItem('kas_9d_total', formatRupiah);
            localStorage.setItem('kas_9d_detail', JSON.stringify(dataKasSiswa));

            alert(`Total kas Rp ${formatRupiah} berhasil diupdate ke Dashboard!`);
        }

        function muatDataKas() {
            const totalSimpanan = localStorage.getItem('kas_9d_total');
            if (totalSimpanan) {
                document.getElementById('stat-kas').innerText = totalSimpanan;
            }

            const detailKas = JSON.parse(localStorage.getItem('kas_9d_detail')) || [];
            if (detailKas.length > 0) {
                // Beri jeda sedikit agar DOM tabel siap
                setTimeout(() => {
                    for (let i = 0; i < detailKas.length; i++) {
                        if(detailKas[i] > 0) {
                            document.getElementById(`kas_${i+1}`).value = detailKas[i];
                        }
                    }
                }, 100);
            }
        }
    </script>
</body>
</html>
