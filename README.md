Nama: Rezha Amelia Putri Firmansyah
Npm: 07352311127
Kelas : 5IF1 

import'dart:math';

// CLASS ABSTRAK 
abstract class Transportasi {
  final String id;
  final String nama;
  final double _tarifDasar;
  final int kapasitas;
  Transportasi(this.id, this.nama, this._tarifDasar, this.kapasitas);

//POLYMORPHISM method abstrac
double hitungTarif(int jumlahPenumpang);
double get tarifDasar =>_tarifDasar; //Getter
}

// INHERITANCE DAN POLYMORPHISM
// Taksi (Tarif berdasarkan jarak)
class Taksi extends Transportasi {
  final double jarak;
  Taksi(String id, String nama, double tarifDasar, int kapasitas, this.jarak)
  :super(id, nama, tarifDasar, kapasitas);

  double hitungTarif(int jumlahPenumpang) {
    if (jumlahPenumpang > kapasitas) return 0.0;
    return _tarifDasar * jarak;
  }
}
//Bus (Tarif Penumpang dan Wifi)
class Bus extends Transportasi {
  final bool adaWifi;
  Bus(String id, String nama, double tarifDasar, int kapasitas, this.adaWifi)
  :super(id, nama, tarifDasar, kapasitas);

  double hitungTarif(int jumlahPenumpang) {
    if (jumlahPenumpang > kapasitas) return 0.0;
    double tarif =_tarifDasar * jumlahPenumpang;
    return tarif + (adaWifi ? 5000 : 0);
  }
}
// Pesawat (Tarif berdasarkan Penumpang dan pengali Kelas)
class Pesawat extends Transportasi {
  final String kelas;
  Pesawat(String id, String nama, double tarifDasar, int kapasitas, this.kelas)
  :super(id, nama, tarifDasar, kapasitas);

  double hitungTarif(int jumlahPenumpang) {
    if (jumlahPenumpang > kapasitas) return 0.0;
    double multiplier = (kelas.toLowerCase() == "bisnis") ? 1.5 : 1.0;
    return _tarifDasar * jumlahPenumpang * multiplier;
  }
}

// KELAS PEMESANAN
class Pemesanan {
  final String idPemesanan;
  final String namaPelanggan;
  final Transportasi transportasi;
  final double totalTarif;
  Pemesanan(this.idPemesanan, this.namaPelanggan, this.transportasi, this.totalTarif);
  
  void cetakStruk() {
    print(' Struk ${idPemesanan} ({transportasi.nama}) : Rp${totalTarif.toStringAsFixed(0)}');
  }
  Map<String, dynamic> toMap() {
  return {'id': idPemesanan, 'pelanggan': namaPelanggan, 'tarif': totalTarif};
  }
}
PemesananbuatPemesanan(Transportasi t, String nama, int jumlahPenumpang) {
  double total = t.hitungTarif(jumlahPenumpang);
  String id = 'SR' + Random().nextInt(99999).toString().padLeft(5, '0');
  return Pemesanan(id, nama, t, total);
}

void tampilSemuaPemesanan(List<Pemesanan> daftar) {
  print('\n=== RIWAYAT PEMESANAN ===');
  if (daftar.isEmpty) return;
  for (var i = 0; i < daftar.length; i++) {
    var p = daftar[i];
    print ('${i + 1}. (${p.idPemesanan} ${p.namaPelanggan} ${p.transportasi.nama}) = Rp${ p.totalTarif.toStringAsFixed(0)}');
  }
}

//FUNGSI MAIN
void main() {
  //membuat objek transportasi
  var taksi = Taksi('TX101', 'Taksi Premiun', 15000.0, 4, 12.0);
  var bus = Bus('BS202', 'Bus Execisuve', 10000.0, 50, true);
  var pesawat = Pesawat('PS3013', 'Garuda', 900000.0, 180, 'Ekonomi');

  //Penyimpanan semua data
  List<Pemesanan> daftarPesanan =[];

  //membuat Pesanan dan menyimpan ke list
  print('--- Transaksi Dibuat ---');

  var p1 = PemesananbuatPemesanan (taksi, 'Andi', 3);
  p1.cetakStruk();

  var p2 = PemesananbuatPemesanan (bus, 'Lala', 10);
  p2.cetakStruk();

  var p3 = PemesananbuatPemesanan (pesawat, 'Toni', 1);
  p3.cetakStruk();

  daftarPesanan.add(p1);
  daftarPesanan.add(p2);
  daftarPesanan.add(p3);

  //Menampilkan semua hasil
  tampilSemuaPemesanan(daftarPesanan);
}
