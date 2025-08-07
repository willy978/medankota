<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Peta Instansi & Perumahan Medan (Full Data Real File)</title>
  <style>
    body { font-family: Arial, sans-serif; padding:18px;}
    #map { width: 100%; height: 520px; margin: 20px 0 10px 0; border-radius: 10px;}
    #dropdowns {margin-bottom: 0;}
    label { font-weight: bold; margin-right: 10px;}
    select { min-width:220px; margin-right: 15px; margin-bottom:10px;}
    #distanceInfo { margin-bottom:10px; font-weight:bold; }
    .info-table { margin-top: 12px;}
    table { border-collapse: collapse; width: 100%; max-width:700px;}
    th, td { border: 1px solid #ccc; padding: 6px 8px; text-align:left;}
    th { background: #f7f7f7;}
    h4 { margin-bottom:0.4em;}
  </style>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
</head>
<body>
  <h3>Peta Instansi & Perumahan Kota Medan</h3>
  <div id="dropdowns">
    <label for="startingPoint">Starting Point</label>
    <select id="startingPoint"></select>
    <label for="destination">Destination</label>
    <select id="destination"></select>
  </div>
  <div id="distanceInfo"></div>
  <div id="map"></div>
  <div class="info-table" id="infoTable"></div>

<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
<script>
// ======= DATA ASLI DARI FILE UPLOAD (Gabungan_Final_Nett, perumahan) =======
const perusahaan = [
{"Nama Perusahaan":"Badan Arkeologi Sumut","Alamat":"Jl. Seroja Raya Gg. Arkeologi No. 1, Tanjung Selamat, Medan Tuntungan, Medan 20134","Latitude":3.535294510,"Longitude":98.60667976},
{"Nama Perusahaan":"Balai Bahasa Sumatera Utara","Alamat":"Jl. Kolam No.7, Kenangan Baru, Kec. Percut Sei Tuan, Kota Medan, Sumatera Utara 20371","Latitude":3.600749442,"Longitude":98.72291762},
{"Nama Perusahaan":"Balai Besar Guru Penggerak","Alamat":"Jl. Kenanga Raya No.64, Tj. Sari, Kec. Medan Selayang, Kota Medan, Sumatera Utara 20122","Latitude":3.561922694,"Longitude":98.63034642},
{"Nama Perusahaan":"Balai Diklat Industri Medan","Alamat":"Jl. Damai No.32, Timbang Deli, Kec. Medan Amplas, Kota Medan, Sumatera Utara 20149","Latitude":3.536686773,"Longitude":98.73275438},
{"Nama Perusahaan":"Balai Harta Peninggalan Medan","Alamat":"Jl. Listrik No.10, Petisah Tengah, Kec. Medan Petisah, Kota Medan, Sumatera Utara 20112","Latitude":3.585927729,"Longitude":98.67579294},
{"Nama Perusahaan":"Balai Pengembangan Kompetensi PUPR Wilayah I Medan","Alamat":"Jl. Sakti Lubis No.7A, Siti Rejo I, Kec. Medan Kota, Kota Medan, Sumatera Utara 20219","Latitude":3.555002071,"Longitude":98.68995627},
{"Nama Perusahaan":"Balai Besar Wilayah Sungai Sumatera II","Alamat":"Jl. Jenderal Besar A.H. Nasution No.30, Pangkalan Masyhur, Kec. Medan Johor, Kota Medan, Sumatera Utara 20143","Latitude":3.542240288,"Longitude":98.67399481},
{"Nama Perusahaan":"BBPVP Medan","Alamat":"Jl. Amal No.9, Lalang, Medan, Kota Medan, Sumatera Utara 20126","Latitude":3.585579695,"Longitude":98.61513554},
{"Nama Perusahaan":"BKKBN SUMUT","Alamat":"Jl. Gunung Krakatau No.110, Pulo Brayan Darat II, Kec. Medan Tim., Kota Medan, Sumatera Utara 20239","Latitude":3.628880168,"Longitude":98.68088543},
{"Nama Perusahaan":"BNN Prov. Sumut","Alamat":"Jl. Balai Pom No.1 Blok A, Medan Estate, Percut Sei Tuan, Deli Serdang Regency, North Sumatra 20371","Latitude":3.605640768,"Longitude":98.71803282},
{"Nama Perusahaan":"BPJS Kesehatan Medan","Alamat":"Jl. Karya No.135, Karang Berombak, Kec. Medan Barat., Kota Medan, Sumatera Utara 20117","Latitude":3.613478069,"Longitude":98.66517250},
{"Nama Perusahaan":"BPJS Ketenagakerjaan Medan Kota","Alamat":"Jl. Kapten Patimura No.334 Lantai 1, Darat, Kec. Medan Baru, Kota Medan, Sumatera Utara 20153","Latitude":3.575694385,"Longitude":98.66575341},
{"Nama Perusahaan":"BPS Kota Medan","Alamat":"Jl. Gaperta No.311, Helvetia, Kec. Medan Helvetia, Kota Medan, Sumatera Utara 20123","Latitude":3.604963973,"Longitude":98.62923738},
{"Nama Perusahaan":"BSIP Sumatera Utara","Alamat":"Jl. Jenderal Besar A.H. Nasution No.1 B, Pangkalan Masyhur, Kec. Medan Johor, Kota Medan, Sumatera Utara 20143","Latitude":3.541588660,"Longitude":98.67720966},
{"Nama Perusahaan":"Dinas Perhubungan Kota Medan","Alamat":"Jl. Pinang Baris, Lalang, Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20127","Latitude":3.588463574,"Longitude":98.60830143},
{"Nama Perusahaan":"Kanwil DJKN Sumatera Utara","Alamat":"Medan Unit II, Gedung Keuangan Negara, Jl. Pangeran Diponegoro No.30A Lt. 4, Madras Hulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20151","Latitude":3.579081575,"Longitude":98.67142937},
{"Nama Perusahaan":"Dirjen Bea dan Cukai Kanwil Sumut","Alamat":"Gedung Keuangan Negara Medan II, Pangeran Diponegoro No. 30A, Medan Sulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20152","Latitude":3.578963183,"Longitude":98.67201591},
{"Nama Perusahaan":"PLN Icon Plus SBU Regional Sumbagut","Alamat":"Jalan Brigjend Katamso Km. 5,5 No. 30, RW.5, Titi Kuning, Kec. Medan Johor, Kota Medan, Sumatera Utara","Latitude":3.539361781,"Longitude":98.68607211},
{"Nama Perusahaan":"Indonesia Power (Belawan)","Alamat":"Jl. Pulau Sicanang No.1, Belawan Pulau Sicanang, Medan Kota Belawan, Sumatera Utara 20414","Latitude":3.776191991,"Longitude":98.67155525},
{"Nama Perusahaan":"Jasa Marga (Cabang Belmera Medan)","Alamat":"Jl. Alumunium Raya, Tj. Mulia, Kec. Medan Deli, Kota Medan, Sumatera Utara 20241","Latitude":3.642518116,"Longitude":98.68234947},
{"Nama Perusahaan":"Kantor Imigrasi Kelas I TPI Polonia","Alamat":"Jl. Mangkubumi No.2, A U R, Kec. Medan Maimun, Kota Medan, Sumatera Utara 20214","Latitude":3.583829246,"Longitude":98.67900817},
{"Nama Perusahaan":"Kantor Pengawasan & Pelayanan BC Medan (Main Office)","Alamat":"Bandar Udara Internasional Kualanamu, Area Perkantoran, Ps. Enam Kuala Namu, Kec. Beringin, Kabupaten Deli Serdang, Sumatera Utara 20552","Latitude":3.631967000,"Longitude":98.87173809},
{"Nama Perusahaan":"Kantor Pengawasan & Pelayanan BC Medan","Alamat":"Jl. Suwondo No.1, Suka Damai, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20212","Latitude":3.566003268,"Longitude":98.67150990},
{"Nama Perusahaan":"Kantor Wilayah Telekomunikasi Telkom Indonesia","Alamat":"Jl. Prof. H. M. Yamin No.13, Perintis, Kec. Medan Timur., Kota Medan, Sumatera Utara 20235","Latitude":3.594435018,"Longitude":98.67947346},
{"Nama Perusahaan":"Kanwil DJP Sumut","Alamat":"Jl. Suka Mulia No.17A, A U R, Kec. Medan Maimun, Kota Medan, Sumatera Utara 20151","Latitude":3.582955375,"Longitude":98.67819066},
{"Nama Perusahaan":"Kanwil Ditjen PBN Provinsi Sumatera Utara","Alamat":"Jl. Pangeran Diponegoro No.30a, Madras Hulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20152","Latitude":3.579010929,"Longitude":98.67187033},
{"Nama Perusahaan":"Kantor Wilayah Kementerian Agama Provinsi Sumatera Utara","Alamat":"Jl. Gatot Subroto No.261, Lalang, Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20127","Latitude":3.593170911,"Longitude":98.62079610},
{"Nama Perusahaan":"Kemenag Kota Medan (MAN/MIN/MTSN)","Alamat":"Jl. Sei Batu Gingging Ps. X No.12, Merdeka, Kec. Medan Baru, Kota Medan, Sumatera Utara 20153","Latitude":3.574941979,"Longitude":98.65515886},
{"Nama Perusahaan":"Konservasi Sumber Daya Alam (BKSDA) Sumatera Utara","Alamat":"Jl. Sisingamangaraja No.14, Harjosari II, Kec. Medan Amplas, Kota Medan, Sumatera Utara 20217","Latitude":3.544312543,"Longitude":98.69820243},
{"Nama Perusahaan":"KPP Medan Belawan","Alamat":"JL Kolonel Laut Jl. KL. Yos Sudarso No.27 KM 8, RW.2, Tj. Mulia, Kec. Medan Deli, Kota Medan, Sumatera Utara 20241","Latitude":3.650051834,"Longitude":98.66269316},
{"Nama Perusahaan":"KPP Medan II","Alamat":"Jl. Hang Tuah, Madras Hulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara","Latitude":3.578212980,"Longitude":98.67144660},
{"Nama Perusahaan":"KPP Medan Polonia","Alamat":"Jl. Suka Mulia No.17A, A U R, Kec. Medan Maimun, Kota Medan, Sumatera Utara 20151","Latitude":3.582822370,"Longitude":98.67832614},
{"Nama Perusahaan":"KPP Medan Timur","Alamat":"Gedung Kanwil DJP Sumatera Utara I Lt. I dan Lt. IV, Jl. Suka Mulia No.17A, A U R, Kec. Medan Maimun, Kota Medan, Sumatera Utara 20151","Latitude":3.582622259,"Longitude":98.67819340},
{"Nama Perusahaan":"KPP Pratama Medan Polonia","Alamat":"Jl. Suka Mulia No.17A, A U R, Kec. Medan Maimun, Kota Medan, Sumatera Utara 20151","Latitude":3.58281906,"Longitude":98.67834042},
{"Nama Perusahaan":"KPP Pratama Lubuk Pakam","Alamat":"Gedung Keuangan Negara, Lantai 2 dan 4, Jl. Pangeran Diponegoro No.30A, Madras Hulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20152","Latitude":3.579495978,"Longitude":98.67195311},
{"Nama Perusahaan":"KPPN Medan 1","Alamat":"Jl. Pangeran Diponegoro No.30A, Madras Hulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20152","Latitude":3.578870547,"Longitude":98.67179360},
{"Nama Perusahaan":"KPU Provinsi Sumatera Utara","Alamat":"Jl. Perintis Kemerdekaan No.35, Gaharu, Kec. Medan Timur., Kota Medan, Sumatera Utara 20232","Latitude":3.599136033,"Longitude":98.68308968},
{"Nama Perusahaan":"PT Pelabuhan Indonesia (Persero)","Alamat":"GRHA PELINDO SATU, Jalan Lingkar Pelabuhan No.1, Belawan II, Medan Kota Belawan, Medan City, North Sumatra 20411","Latitude":3.78145392,"Longitude":98.68847529},
{"Nama Perusahaan":"Pengadilan Tata Usaha Negeri Medan","Alamat":"Jl. Bunga Raya No.18, Asam Kumbang, Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20128","Latitude":3.567556001,"Longitude":98.61473487},
{"Nama Perusahaan":"Pengadilan Tinggi Agama Medan","Alamat":"Jl. Kapten Sumarsono No.12, Helvetia Timur., Kec. Medan Helvetia, Kota Medan, Sumatera Utara 20124","Latitude":3.615365831,"Longitude":98.64712142},
{"Nama Perusahaan":"Pengadilan Tinggi Tata Usaha Negara Medan","Alamat":"Medan Estate, Jl. Peratun No.1, Kenangan Baru, Kec. Percut Sei Tuan, Kabupaten Deli Serdang, Sumatera Utara 20371","Latitude":3.610531461,"Longitude":98.71584962},
{"Nama Perusahaan":"Pesantren Ar-Raudlatul Hasanah","Alamat":"Jl. Setia Budi, Simpang Selayang, Kec. Medan Tuntungan, Kota Medan, Sumatera Utara 20135","Latitude":3.524828142,"Longitude":98.62132217},

{"Nama Perusahaan":"PLN Icon Plus SBU Regional Sumbagut","Alamat":"Jalan Brigjend Katamso Km. 5,5 No. 30, RW.5, Titi Kuning, Kec. Medan Johor, Kota Medan, Sumatera Utara","Latitude":3.539361781,"Longitude":98.68607211},
{"Nama Perusahaan":"PLTGU Belawan","Alamat":"Belawan Pulau Sicanang, Medan Kota Belawan, Sumatera Utara 20411","Latitude":3.774143364,"Longitude":98.66851430},
{"Nama Perusahaan":"PT Inalum (Persero)","Alamat":"Jl. R.A. Kartini No.21, Madras Hulu, Kec. Medan Polonia, Kota Medan, Sumatera Utara 20152","Latitude":3.579714004,"Longitude":98.67327193},
{"Nama Perusahaan":"PT Industri Karet Nusantara","Alamat":"Jl. T. R. M Nozama, Medan, Kec. Medan Amplas, 20148","Latitude":3.532360049,"Longitude":98.72787673},
{"Nama Perusahaan":"Kantor PT KAI Divre I Sumatera Utara","Alamat":"Jl. Prof. H. M. Yamin No.13, Gg. Buntu, Kec. Medan Timur., Kota Medan, Sumatera Utara 20236","Latitude":3.593927529,"Longitude":98.67983975},
{"Nama Perusahaan":"PT KAI Balai Yasa","Alamat":"Jl. Bengkel No.1, Pulo Brayan Bengkel, Kec. Medan Timur., Kota Medan, Sumatera Utara 20241","Latitude":3.636723088,"Longitude":98.67316039},
{"Nama Perusahaan":"Pertamina Region I","Alamat":"Jl. KL. Yos Sudarso No.8-10, Silalas, Kec. Medan Barat., Kota Medan, Sumatera Utara 20114","Latitude":3.603794358,"Longitude":98.67443776},
{"Nama Perusahaan":"Kantor PGN Glugur","Alamat":"Glugur Kota, Kec. Medan Barat., Kota Medan, Sumatera Utara 20238","Latitude":3.615558793,"Longitude":98.66997620},
{"Nama Perusahaan":"Perusahaan Gas Negara","Alamat":"Jl. Imam Bonjol No.15D, Petisah Tengah, Kec. Medan Petisah, Kota Medan, Sumatera Utara 20212","Latitude":3.615589267,"Longitude":98.66997295},
{"Nama Perusahaan":"PT. PLN (Persero) UIP3BS UPT Medan","Alamat":"Jl. Listrik No.12, Petisah Tengah, Kec. Medan Petisah, Kota Medan, Sumatera Utara 20112","Latitude":3.587324179,"Longitude":98.67549112},
{"Nama Perusahaan":"PT PLN (Persero) UID Sumatera Utara","Alamat":"Jl. KL. Yos Sudarso No.284, Glugur Kota, Kec. Medan Bar., Kota Medan, Sumatera Utara 20238","Latitude":3.615750353,"Longitude":98.67266482},
{"Nama Perusahaan":"PT Prima Indonesia Logistik","Alamat":"Jl. Raya Pelabuhan, Pos II Road VI Ujung Baru, Belawan I, Medan Kota Belawan, Kota Medan, Sumatera Utara 20411","Latitude":3.784404594,"Longitude":98.69423817},
{"Nama Perusahaan":"PT Prima Multi Peralatan","Alamat":"Komplek Cemara Asri, Jln. Boulevard Timur No. 28ac 20371, Deliserdang Sumatera Utara","Latitude":3.633606677,"Longitude":98.70391105},
{"Nama Perusahaan":"PT Prima Pengembangan Kawasan","Alamat":"Kantor Pelindo Krakatau Gedung B, Jl. Krakatau Ujung No.100 Lt. 2, Tj. Mulia, Kec. Medan Deli, Kota Medan, Sumatera Utara 20241","Latitude":3.641033048,"Longitude":98.68080151},
{"Nama Perusahaan":"PT Telkom Indonesia Divisi Infratel Sumbagut","Alamat":"Graha Merah Putih, Jl. Putri Hijau No.1 Lt.7, Kesawan, Kec. Medan Barat., Kota Medan, Sumatera Utara 20236","Latitude":3.593203192,"Longitude":98.67610692},
{"Nama Perusahaan":"PT Telkomsel Medan","Alamat":"Graha Merah Putih, Jl. Putri Hijau No.1 Lt.1, Kesawan, Kec. Medan Barat., Kota Medan, Sumatera Utara 20236","Latitude":3.593203192,"Longitude":98.67610692},
{"Nama Perusahaan":"Kantor PT KAI Divre I Sumatera Utara","Alamat":"Jl. Prof. H. M. Yamin No.13, Gg. Buntu, Kec. Medan Timur., Kota Medan, Sumatera Utara 20236","Latitude":3.593759773,"Longitude":98.67985046},
{"Nama Perusahaan":"PTPN II Tanjung Morawa & Galang","Alamat":"Limau Manis, Kec. Tj. Morawa, Kabupaten Deli Serdang, Sumatera Utara","Latitude":3.531744577,"Longitude":98.77936659},
{"Nama Perusahaan":"PTPN III","Alamat":"Jl. Sei Batang Hari No.2, Simpang Tj., Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20122","Latitude":3.586561408,"Longitude":98.64337579},
{"Nama Perusahaan":"PTPN III Kebun Silau Dunia","Alamat":"Jl. Sei Batang Hari No.2, Simpang Tj., Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20122","Latitude":3.586561408,"Longitude":98.64337579},
{"Nama Perusahaan":"Dinas Binamarga Propinsi Sumatera Utara","Alamat":"Jl. Sakti Lubis, Sitirejo II, Kec. Medan Amplas, Kota Medan, Sumatera Utara 20217","Latitude":3.553300966,"Longitude":98.69106464},
{"Nama Perusahaan":"Railink","Alamat":"Jalan Stasiun Kereta Api, Kec. Medan Timur, Kota Medan","Latitude":3.590968682,"Longitude":98.67958557},
{"Nama Perusahaan":"RRI Medan","Alamat":"Jl. Gatot Subroto No.214, Sei Sikambing C. II, Kec. Medan Helvetia, Kota Medan, Sumatera Utara 20123","Latitude":3.591794325,"Longitude":98.63465470},
{"Nama Perusahaan":"RS Siloam Medan","Alamat":"Jl. Imam Bonjol No.6, Petisah Tengah, Kec. Medan Petisah, Kota Medan, Sumatera Utara 20212","Latitude":3.585909856,"Longitude":98.67359852},
{"Nama Perusahaan":"RS Stella Maris Medan","Alamat":"Jl. Samanhudi No.20, J A T I, Kec. Medan Maimun, Kota Medan, Sumatera Utara 20152","Latitude":3.572860188,"Longitude":98.68079980},
{"Nama Perusahaan":"Rudenim Medan","Alamat":"Jl. Selebes, Belawan I, Medan Kota Belawan, Kota Medan, Sumatera Utara 20411","Latitude":3.781485049,"Longitude":98.68695073},
{"Nama Perusahaan":"Rumah Sakit Columbia Asia","Alamat":"Jl. Listrik No.2A, Petisah Tengah, Medan Petisah","Latitude":3.585756401,"Longitude":98.67684509},
{"Nama Perusahaan":"Rumah Sakit Ginjal Rasyidah","Alamat":"Jl. Jend. D.I Panjaitan No.144, Sei Sikambing D, Kec. Medan Petisah, Kota Medan, Sumatera Utara 20111","Latitude":3.586048695,"Longitude":98.65976236},
{"Nama Perusahaan":"Rumah Sakit Hermina Medan","Alamat":"Jl. Asrama II (Sei Sikambing C II), Medan Helvetia, Medan 20123","Latitude":3.595396235,"Longitude":98.62744333},
{"Nama Perusahaan":"Rumah Sakit Imelda","Alamat":"Jl. Bilal No.24, Pulo Brayan Darat I, Kec. Medan Timur., Kota Medan, Sumatera Utara 20239","Latitude":3.622780944,"Longitude":98.67494592},
{"Nama Perusahaan":"Rumah Sakit Umum Latersia","Alamat":"Jl. Soekarno Hatta, Tunggurono, Kec. Binjai Timur., Kota Binjai, Sumatera Utara 20351","Latitude":3.607483063,"Longitude":98.52638111},
{"Nama Perusahaan":"Rumah Sakit Mata 77","Alamat":"Jl. Sei Mencirim No.77, Babura, Kec. Medan Baru, Kota Medan, Sumatera Utara 20154","Latitude":3.580669976,"Longitude":98.65440167},
{"Nama Perusahaan":"Rumah Sakit Mata Medan Baru","Alamat":"Jl. Abdullah Lubis No.67, Merdeka, Kec. Medan Baru, Kota Medan, Sumatera Utara 20222","Latitude":3.576825631,"Longitude":98.65879428},
{"Nama Perusahaan":"Rumah Sakit Umum Mitra Medika Tanjung Mulia","Alamat":"Jl. KL. Yos Sudarso No.KM. 7,5, Tj. Mulia, Kec. Medan Deli, Kota Medan, Sumatera Utara 20241","Latitude":3.644965812,"Longitude":98.66254283},
{"Nama Perusahaan":"Rumah Sakit Mitra Medika Premier","Alamat":"Jl. S. Parman No.234a, Petisah Tengah, Kec. Medan Petisah, Kota Medan, Sumatera Utara 20117","Latitude":3.586331951,"Longitude":98.66694394},
{"Nama Perusahaan":"Rumah Sakit Mitra Sejati","Alamat":"Jl. Jenderal Besar A.H. Nasution No.7, Pangkalan Masyhur, Kec. Medan Johor, Kota Medan, Sumatera Utara 20219","Latitude":3.540995433,"Longitude":98.67989994},
{"Nama Perusahaan":"Rumah Sakit Sari Mutiara Lubuk Pakam","Alamat":"Tj. Garbus Satu, Kec. Lubuk Pakam, Kabupaten Deli Serdang, Sumatera Utara 20518","Latitude":3.554694541,"Longitude":98.87892691},
{"Nama Perusahaan":"Rumah Sakit Setia Budi","Alamat":"Jl.Mesjid No.3, Tj. Rejo, Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20154","Latitude":3.573309004,"Longitude":98.64338927},
{"Nama Perusahaan":"Rumah Sakit Sufina Aziz","Alamat":"Jl. Karya Baru No.1, Helvetia Timur., Kec. Medan Helvetia, Kota Medan, Sumatera Utara 20124","Latitude":3.616743910,"Longitude":98.66300814},
{"Nama Perusahaan":"Balai Besar Taman Nasional Gunung Lauser","Alamat":"Jl. Selamat No.137, Sitirejo III, Kec. Medan Amplas, Kota Medan, Sumatera Utara 20226","Latitude":3.549583586,"Longitude":98.70655495},
{"Nama Perusahaan":"Kantor Taspen Cabang Medan","Alamat":"Jl. H. Adam Malik No.64, Silalas, Kec. Medan Barat., Kota Medan, Sumatera Utara 20235","Latitude":3.602943772,"Longitude":98.66916396},
{"Nama Perusahaan":"Telkom Regional/Akses Medan","Alamat":"Jl. Prof. H. M. Yamin No.2, Kesawan, Kec. Medan Baru, Kota Medan, Sumatera Utara 20236","Latitude":3.592975311,"Longitude":98.67741633},
{"Nama Perusahaan":"UINSU","Alamat":"Jl. William Iskandar Ps. V, Medan Estate, Kec. Percut Sei Tuan, Kabupaten Deli Serdang, Sumatera Utara 20371","Latitude":3.604984731,"Longitude":98.72107867},
{"Nama Perusahaan":"UISU (Univ. Islam Sumut)","Alamat":"Jl. Sisingamangaraja No.Kelurahan, Teladan Barat., Kec. Medan Kota, Kota Medan, Sumatera Utara 20217","Latitude":3.562504205,"Longitude":98.69386385},
{"Nama Perusahaan":"UMSU (Univ. Muhammadiyah Sumut)","Alamat":"Jl. Kapten Muchtar Basri No.3, Glugur Darat II, Kec. Medan Timur., Kota Medan, Sumatera Utara 20238","Latitude":3.614731563,"Longitude":98.67624133},
{"Nama Perusahaan":"UNIMED (Universitas Negeri Medan)","Alamat":"Jl. William Iskandar Ps. V, Kenangan Baru, Kec. Percut Sei Tuan, Kabupaten Deli Serdang, Sumatera Utara 20221","Latitude":3.608275145,"Longitude":98.71696850},
{"Nama Perusahaan":"UMA (Universitas Medan Area)","Alamat":"Jl. Setia Budi No.79 B, Tj. Rejo, Kec. Medan Sunggal, Kota Medan, Sumatera Utara 20112","Latitude":3.576870645,"Longitude":98.64320637},
{"Nama Perusahaan":"Universitas Dharmawangsa","Alamat":"Jl. KL. Yos Sudarso No.224, Glugur Kota, Kec. Medan Barat, Kota Medan, Sumatera Utara 20115","Latitude":3.613749815,"Longitude":98.67347290},
{"Nama Perusahaan":"UPT Asrama Haji Medan","Alamat":"Jl. Jenderal Besar A.H. Nasution No.10, Pangkalan Masyhur, Kec. Medan Johor, Kota Medan, Sumatera Utara","Latitude":3.614719485,"Longitude":98.67473905},
{"Nama Perusahaan":"MIN Kota Rantang","Alamat":"Jl. Rantang, Sei Agul, Kec. Medan Barat, Kota Medan","Latitude":3.540435409,"Longitude":98.6739500}
];


const perumahan = [
{"Nama Perumahan":"Johor Medan","Alamat Lengkap":"JL Merci Raya No. 81, Desa Deli Tua, Kec. Namorambe, Kab. Deli Serdang, Sumut 20356 (Properti1)","Latitude":3.424257956,"Longitude":98.64333209,"Status (Unit)":"Subsidi – 13","Tipe & Harga":"Tipe 36/72 – Rp 150 juta/unit","Pengembang / Asosiasi":"Rolex Dwi Tunggal (PI)"},
{"Nama Perumahan":"Setia Budi Flamboyan","Alamat Lengkap":"JL Flamboyan Raya, Kel. Tanjung Selamat, Medan Tuntungan, Medan 20135 (brighton.co.id)","Latitude":3.544814332,"Longitude":98.61201427,"Status (Unit)":"Komersil – 5","Tipe & Harga":"Tipe 45/90 – Rp 400–500 juta","Pengembang / Asosiasi":"Riviera Village Permai (HIMPERRA)"},
{"Nama Perumahan":"Setiabudi Raya","Alamat Lengkap":"JL Setia Budi No.12, Simpang Selayang, Medan Tuntungan, Kota Medan, Sumatera Utara 20135","Latitude":3.535359595,"Longitude":98.62184505,"Status (Unit)":"Komersil – 6","Tipe & Harga":"Tipe 54/110 – Rp 650 juta","Pengembang / Asosiasi":"Anugerah Bangun Cipta (HIMPERRA)"},
{"Nama Perumahan":"Setiabudi Anggrek","Alamat Lengkap":"JL Anggrek Raya, Setia Budi, Simpang Selayang, Kota Medan, Sumatera Utara 20135","Latitude":3.533165986,"Longitude":98.62052604,"Status (Unit)":"Komersil – 6","Tipe & Harga":"Tipe 54/110 – Rp 650 juta","Pengembang / Asosiasi":"Anugerah Bangun Cipta (HIMPERRA)"},
{"Nama Perumahan":"Givenchy One Tahap IV","Alamat Lengkap":"JL Gaperta Ujung, Komplek Givenchy One, Kel. Tanjung Gusta, Kec. Medan Helvetia, Medan","Latitude":3.586400005,"Longitude":98.61841328,"Status (Unit)":"Komersil – 44","Tipe & Harga":"Tipe 60/120 – Rp 850 juta","Pengembang / Asosiasi":"Gaperta Wira Kencana (REI)"},
{"Nama Perumahan":"Podomoro City Deli Medan","Alamat Lengkap":"JL Putri Hijau No.1, Kel. Kesawan, Kec. Medan Barat, Medan","Latitude":3.5940187896961078,"Longitude":98.67422623441794,"Status (Unit)":"Komersil – 510+","Tipe & Harga":"Tipe 70/140 – Rp 900 juta–1.2 m","Pengembang / Asosiasi":"Sinar Medan Deli (REI)"},
{"Nama Perumahan":"Medan Resort City – Amsterdam","Alamat Lengkap":"Jalan Karya Jaya Komplek Medan Resort City Blok A9/07 Merci Barn, Deli Tua, Namo Rambe, Deli Serdang, No. Barn Sumatera Utara","Latitude":3.510588091,"Longitude":98.67475195,"Status (Unit)":"Komersil – 50+","Tipe & Harga":"Tipe 70/140 – Rp 950 juta","Pengembang / Asosiasi":"Bale Dipa Aruna (REI)"},
{"Nama Perumahan":"Medan Resort City – Volendam","Alamat Lengkap":"Jalan Karya Jaya Komplek Medan Resort City Blok A9/07 Merci Barn, Deli Tua, Namo Rambe, Deli Serdang, No. Barn Sumatera Utara","Latitude":3.510588914,"Longitude":98.67475191,"Status (Unit)":"Komersil – 50+","Tipe & Harga":"Tipe 50/100 – Rp 700 juta+","Pengembang / Asosiasi":"Bale Dipa Aruna (REI)"},
{"Nama Perumahan":"Medan Resort City – Cluster K","Alamat Lengkap":"Jalan Karya Jaya Komplek Medan Resort City Blok A9/07 Merci Barn, Deli Tua, Namo Rambe, Deli Serdang, No. Barn Sumatera Utara","Latitude":3.510589114,"Longitude":98.67475192,"Status (Unit)":"Komersil – 50+","Tipe & Harga":"Tipe 80/120 – Rp 800 juta+","Pengembang / Asosiasi":"Bale Dipa Aruna (REI)"},
{"Nama Perumahan":"Deli Asri","Alamat Lengkap":"Jalan Besar Delitua, Tanjah Mijur, Komplek Deli Asri No. 40 Sumatera Utara, Kab Deli Serdang, Sibiru-Biru, Sidomulyo","Latitude":3.495003798,"Longitude":98.65037986,"Status (Unit)":"Subsidi – 5","Tipe & Harga":"Tipe 36/72 – Rp 150 juta","Pengembang / Asosiasi":"Gunung Permata Indah (REI)"},
{"Nama Perumahan":"Villa Patumbak Permai","Alamat Lengkap":"Jalan Pelajar Ujung No. 1 Sumatera Utara, Kab. Deli Serdang, Patumbak, Marendal I","Latitude":3.499754441,"Longitude":98.67141805,"Status (Unit)":"Subsidi – 53; Komersil – 8","Tipe & Harga":"Tipe 36/72 – Rp 150 juta; Tipe 54/110 – Rp 600 juta","Pengembang / Asosiasi":"Patumbak Permai Mas (APERSI)"},
{"Nama Perumahan":"Cluster Taman Deli Kencana","Alamat Lengkap":"Jalan Karya Jaya deli kencana, JL. Cayas Raya No.9, Komplek Deli Kencana, Sumatera Utara 20356","Latitude":3.483742093,"Longitude":98.65798711,"Status (Unit)":"Subsidi – 5","Tipe & Harga":"Tipe 36/72 – Rp 150 juta","Pengembang / Asosiasi":"Hijau Tapak Mandiri (APERSI)"},
{"Nama Perumahan":"Griya Makmur 8","Alamat Lengkap":"JL Sugeng, Sumber Rejo Tim., Kec. Binjai Selatan, Kabupaten Deli Serdang, Sumatera Utara 20371","Latitude":3.613603724,"Longitude":98.67596207,"Status (Unit)":"Subsidi – 5","Tipe & Harga":"Tipe 36/72 – Rp 150 juta","Pengembang / Asosiasi":"Cahaya Terang Sahabat (REI)"},
{"Nama Perumahan":"Sicoland Hill Delitua","Alamat Lengkap":"JL Banjaran, Sidomulyo, Kec. Sibiru-biru, Kabupaten Deli Serdang, Sumatera Utara 20355, Indonesia","Latitude":3.502174508,"Longitude":98.65198607,"Status (Unit)":"Subsidi – 89","Tipe & Harga":"Tipe 36/72 – Rp 150 juta","Pengembang / Asosiasi":"ANF Sejahtera Abadi (HIMPERRA)"},
{"Nama Perumahan":"Graha Mutiara Tirta Deli","Alamat Lengkap":"JL Tirta Deli, Limau Manis, Kec. Tj. Morawa, Kabupaten Deli Serdang, Sumatera Utara 20362, Indonesia","Latitude":3.512580135,"Longitude":98.73826109,"Status (Unit)":"Subsidi – 5","Tipe & Harga":"Tipe 36/72 – Rp 150 juta","Pengembang / Asosiasi":"Madya Kreasi Lestari (REI)"},
{"Nama Perumahan":"Puri Asri Taramedang Residence IV","Alamat Lengkap":"JL Ujung Serdang, Patumbak Dua, Kec. Patumbak, Kabupaten Deli Serdang, Sumatera Utara 20362, Indonesia","Latitude":3.505110918,"Longitude":98.68725003,"Status (Unit)":"Subsidi – 5","Tipe & Harga":"Tipe 36/72 – Rp 150 juta","Pengembang / Asosiasi":"Permunas"},
{"Nama Perumahan":"Taman Putri Deli Namorambe","Alamat Lengkap":"JL Durian III No. 106 Sumatera Utara, Deli Tua","Latitude":3.484870162,"Longitude":98.68309411,"Status (Unit)":"Subsidi – 46","Tipe & Harga":"Tipe 60/120 – Rp 150 juta","Pengembang / Asosiasi":"Manunggal Makmur Sejahtera (REI)"},
{"Nama Perumahan":"Jaharun Indah Residence","Alamat Lengkap":"Jalan Besar Petumbukan Kecamatan Deliserdang no. 01 SUMATERA UTARA, KAB DELI SERDANG, Galang","Latitude":3.588618321,"Longitude":98.67932459,"Status (Unit)":"Subsidi – 5","Tipe & Harga":"Tipe 53/162 – Rp 1,5 m","Pengembang / Asosiasi":"Virland"},
{"Nama Perumahan":"Halton Place","Alamat Lengkap":"JL Panglima Denai, Medan Denai, Kota Medan, Sumatera Utara 20226","Latitude":3.610184346,"Longitude":98.66184384,"Status (Unit)":"Komersil – 23","Tipe & Harga":"Tipe 84/192,6 – Rp1,75 m","Pengembang / Asosiasi":"Greenland Garden Realty"},
{"Nama Perumahan":"Vienna Botanical Living","Alamat Lengkap":"JL Jamin Ginting Km. 12 NO. – Sumatera Utara, Medan Tuntungan, Lauchi","Latitude":3.510852961,"Longitude":98.64574165,"Status (Unit)":"Komersil – 57","Tipe & Harga":"Tipe 198/216; 3 mp; Tipe 123/153 - Rp2,7 m","Pengembang / Asosiasi":"Majainto Citra Swakarsa"},
{"Nama Perumahan":"The Grandis","Alamat Lengkap":"JL Jamin Ginting Km 8,5 No.10, Kota Medan, Medan Tuntungan, Mangga","Latitude":3.510668984,"Longitude":98.64765911,"Status (Unit)":"Komersil – 57","Tipe & Harga":"Tipe 198/216; 3 mp; Tipe 123/153 - Rp2,7 m","Pengembang / Asosiasi":"Majainto Citra Swakarsa"},
{"Nama Perumahan":"Kitakaze Residence","Alamat Lengkap":"JL Jamin Ginting Km 12,5 Villa Zeqita No.1 Sumatera Utara, Kota Medan, Medan Tuntungan, Lauchi","Latitude":3.508631346,"Longitude":98.64174419,"Status (Unit)":"Komersil – 5","Tipe & Harga":"Tipe 105/98 - Rp979 Juta; Tipe 66/98 - Rp755 Jt","Pengembang / Asosiasi":"Batu Piher Sibero"}
];
// === Custom marker icon
const iconPerusahaan = L.icon({
  iconUrl: 'https://cdn.jsdelivr.net/gh/pointhi/leaflet-color-markers@master/img/marker-icon-blue.png',
  iconSize: [25,41], iconAnchor:[12,41], popupAnchor:[1,-34]
});
const iconPerumahan = L.icon({
  iconUrl: 'https://cdn.jsdelivr.net/gh/pointhi/leaflet-color-markers@master/img/marker-icon-orange.png',
  iconSize: [25,41], iconAnchor:[12,41], popupAnchor:[1,-34]
});
const iconSelect = L.icon({
  iconUrl: 'https://cdn.jsdelivr.net/gh/pointhi/leaflet-color-markers@master/img/marker-icon-red.png',
  iconSize: [27,45], iconAnchor:[13,44], popupAnchor:[1,-34]
});

// ==== Populate Dropdowns
const sp = document.getElementById('startingPoint');
const dst = document.getElementById('destination');
sp.innerHTML = perusahaan.map((d,i) =>
  `<option value="${i}">${d['Nama Perusahaan']}</option>`
).join('');
dst.innerHTML = perumahan.map((d,i) =>
  `<option value="${i}">${d['Nama Perumahan']}</option>`
).join('');

// ==== Inisialisasi Map
const map = L.map('map').setView([3.585,98.67], 12);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

// ==== Plot semua marker + simpan ke array untuk highlight
const perusahaanMarkers = [];
perusahaan.forEach((d,i) => {
  if(d.Latitude && d.Longitude) {
    const m = L.marker([parseFloat(d.Latitude),parseFloat(d.Longitude)],{icon:iconPerusahaan})
      .addTo(map)
      .bindPopup(`<b>${d['Nama Perusahaan']}</b><br>${d['Alamat']}`);
    perusahaanMarkers.push(m);
  }
});
const perumahanMarkers = [];
perumahan.forEach((d,i) => {
  if(d.Latitude && d.Longitude) {
    const m = L.marker([parseFloat(d.Latitude),parseFloat(d.Longitude)],{icon:iconPerumahan})
      .addTo(map)
      .bindPopup(`<b>${d['Nama Perumahan']}</b><br>${d['Alamat Lengkap']}<br>
      <b>Status Unit:</b> ${d['Status (Unit)']}<br>
      <b>Tipe & Harga:</b> ${d['Tipe & Harga']}<br>
      <b>Pengembang/Asosiasi:</b> ${d['Pengembang / Asosiasi']}`);
    perumahanMarkers.push(m);
  }
});

// ==== Highlight & Jarak Dropdown
let selMarker1 = null, selMarker2 = null, routeLine = null;
function calcDistance(lat1, lon1, lat2, lon2) {
  function toRad(x) { return x * Math.PI / 180; }
  const R = 6371e3;
  const φ1 = toRad(lat1), φ2 = toRad(lat2);
  const Δφ = toRad(lat2-lat1), Δλ = toRad(lon2-lon1);
  const a = Math.sin(Δφ/2) * Math.sin(Δφ/2) +
            Math.cos(φ1) * Math.cos(φ2) *
            Math.sin(Δλ/2) * Math.sin(Δλ/2);
  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
  return R * c; // meter
}
function updateInfo() {
  const pi = sp.selectedIndex;
  const di = dst.selectedIndex;
  const p = perusahaan[pi];
  const r = perumahan[di];
  // Remove highlight marker/line
  if(selMarker1) { map.removeLayer(selMarker1); selMarker1=null; }
  if(selMarker2) { map.removeLayer(selMarker2); selMarker2=null; }
  if(routeLine) { map.removeLayer(routeLine); routeLine=null; }
  // Highlight selected
  if (p && r && p.Latitude && r.Latitude) {
    selMarker1 = L.marker([parseFloat(p.Latitude), parseFloat(p.Longitude)],{icon:iconSelect}).addTo(map)
      .bindPopup(`<b>${p['Nama Perusahaan']}</b><br>${p['Alamat']}`).openPopup();
    selMarker2 = L.marker([parseFloat(r.Latitude), parseFloat(r.Longitude)],{icon:iconSelect}).addTo(map)
      .bindPopup(`<b>${r['Nama Perumahan']}</b><br>${r['Alamat Lengkap']}<br>
        <b>Status Unit:</b> ${r['Status (Unit)']}<br>
        <b>Tipe & Harga:</b> ${r['Tipe & Harga']}<br>
        <b>Pengembang/Asosiasi:</b> ${r['Pengembang / Asosiasi']}`).openPopup();
    routeLine = L.polyline([[parseFloat(p.Latitude),parseFloat(p.Longitude)],[parseFloat(r.Latitude),parseFloat(r.Longitude)]], {color:'red',weight:3,dashArray:'5,7'}).addTo(map);
    map.fitBounds([[parseFloat(p.Latitude),parseFloat(p.Longitude)],[parseFloat(r.Latitude),parseFloat(r.Longitude)]],{padding:[40,40]});
    // Distance
    const dist = calcDistance(parseFloat(p.Latitude), parseFloat(p.Longitude), parseFloat(r.Latitude), parseFloat(r.Longitude));
    document.getElementById('distanceInfo').innerHTML = 
      `Jarak antar titik: <b>${dist>1000?(dist/1000).toFixed(2)+' km':Math.round(dist)+' meter'}</b>`;
    // Table info
    let html = `<h4>Detail Starting Point</h4>
    <table>
      <tr><th>Nama Perusahaan</th><td>${p['Nama Perusahaan']}</td></tr>
      <tr><th>Alamat</th><td>${p['Alamat']}</td></tr>
    </table>
    <h4>Detail Destination</h4>
    <table>
      <tr><th>Nama Perumahan</th><td>${r['Nama Perumahan']}</td></tr>
      <tr><th>Alamat Lengkap</th><td>${r['Alamat Lengkap']}</td></tr>
      <tr><th>Status Unit</th><td>${r['Status (Unit)']}</td></tr>
      <tr><th>Tipe & Harga</th><td>${r['Tipe & Harga']}</td></tr>
      <tr><th>Pengembang/Asosiasi</th><td>${r['Pengembang / Asosiasi']}</td></tr>
    </table>`;
    document.getElementById('infoTable').innerHTML = html;
  } else {
    document.getElementById('distanceInfo').innerHTML = '';
    document.getElementById('infoTable').innerHTML = '';
    map.setView([3.585,98.67], 12);
  }
}



sp.addEventListener('change', updateInfo);
dst.addEventListener('change', updateInfo);
updateInfo();
</script>
</body>
</html>
