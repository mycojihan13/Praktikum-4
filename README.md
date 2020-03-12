<!-- MULAI -->

<!-- Source Rest API GitHub -->
# ci-restserver
Check the recent version at https://github.com/chriskacerguis/codeigniter-restserver

My alternate version https://github.com/ardisaurus/old-rest-ci

<!-- Pengertian Rest API -->

- REST (Representional State Transfer) adalah suatu arsitektur metode komunikasi yang menggunakan protokol HTTP untuk pertukaran data dan metode ini sering diterapkan dalam pengembangan aplikasi. Dimana tujuannya adalah untuk menjadikan sistem yang memiliki performa yang baik, cepat dan mudah untuk di kembangkan (scale) terutama dalam pertukaran dan komunikasi data

- Pada arsitektur REST, REST server menyediakan resources (sumber daya/data) dan REST client mengakses dan menampilkan resource tersebut untuk penggunaan selanjutnya.

- Untuk memungkinkan komunikasi data antara client dan server, codeigniter membutuhkan library tambahan berupa library curl. Curl adalah sebuah program yang memungkinan kita memindai data dari atau ke sebuah server. Salah satu library curl yang dapat digunakan adalah library curl dari Phil Sturgeon yang terdapat pada folder application/libraries/REST_Controller.php.

<!-- Instalasi Rest API di CodeIgniter -->
<!-- Persiapan -->

Dalam pembuatan Rest API Server ini dibutuhkan:
1. Webserver seperti XAMPP, WAMPP, dll

2. CodeIgniter dan library Rest Server seperti Library Curl yang dibuat oleh Phil Sturgeon seperti yang telah disebutkan diatas

Setelah semuanya siap, kita akan masuk langkah-langkah nya, let's go!
<!-- Langkah-langkah -->
JANGAN LUPA ENABLE AUTOLOAD LIBRARIES = 'database'!

1. Extract CodeIgniter dan Library Rest Server dan pindah ke folder htdocs supaya terdetect oleh webserver XAMPP kita. Silakan rename folder jika diperlukan

2. Masukkan http://localhost/ci-restserver-master/index.php/rest_server pada adress bar browser kita
3. Kemudian, konfigurasi database kita, sebagai contoh, kita buat database dengan nama 'kontak'

4. Selanjutnya, kita buat tabel baru bernama 'telepon' 

5. Isi tabel 'telepon' dengan kolom 'id', 'nama', 'nomor'

6. Setelah itu, buka database.php pada folder /application/config dan isikan field:
'username' = 'root'
'database' = 'kontak'

Sekarang kita masuk pada intinya Rest API yaitu fungsi GET, POST, dan PUT 
Proses ini dilakukan menggunakan aplikasi pihak ketiga yaitu Postman for Windows

<!-- GET -->

7. Buat file php baru di folder application/controllers dengan nama kontak.php
Tambahkan sebuah kode fungsi get pada kontak.php, silakan cek application/controllers/kontak.php untuk keterangan lebih lanjut.

    <?php

defined('BASEPATH') OR exit('No direct script access allowed');

require APPPATH . '/libraries/REST_Controller.php';
use Restserver\Libraries\REST_Controller;

class Kontak extends REST_Controller {

    function __construct($config = 'rest') {
        parent::__construct($config);
        $this->load->database();
    }

    //Menampilkan data kontak
    function index_get() {
        $id = $this->get('id');
        if ($id == '') {
            $kontak = $this->db->get('telepon')->result();
        } else {
            $this->db->where('id', $id);
            $kontak = $this->db->get('telepon')->result();
        }
        $this->response($kontak, 200);
    }

    //Masukan function selanjutnya disini
}
?>

Sekarang kita masuk pada aplikasi Postman

8. Untuk menguji kode yang telah dibuat buka Postman, Pilih metode GET, masukan http://localhost/ci-restserver-master/index.php/kontak pada address bar lalu klik "Send". Maka akan muncul hasil GET semua data pada database 'kontak'

9. Kita juga bisa GET (menampilkan) salah satu data dengan menggunakan 'id', masukkan http://localhost/ci-restserver-master/index.php/kontak?id=1. Maka akan muncul data dengan 'id'= 1 saja. 

<!-- POST -->

10. Tambahkan sebuah kode fungsi post pada kontak.php, silakan cek application/controllers/kontak.php untuk keterangan lebih lanjut.
     //Mengirim atau menambah data kontak baru
    function index_post() {
        $data = array(
                    'id'           => $this->post('id'),
                    'nama'          => $this->post('nama'),
                    'nomor'    => $this->post('nomor'));
        $insert = $this->db->insert('telepon', $data);
        if ($insert) {
            $this->response($data, 200);
        } else {
            $this->response(array('status' => 'fail', 502));
        }
    }

    //Masukan function selanjutnya disini

11. Untuk mengujinya buka Postman, pilih metode POST, masukan http://localhost/ci-restserver-master/index.php/kontak pada address bar, klik "Body" pada menu dibawah address bar, pilih x-www-form-urlencoded, masukan key dan value yang diperlukan (id, nama, nomor), lalu klik "Send"

12. Kemudian, lakukan metode get untuk melihat data baru

<!-- PUT -->

13. Tambahkan sebuah kode fungsi put pada kontak.php, silakan cek application/controllers/kontak.php untuk keterangan lebih lanjut

    //Memperbarui data kontak yang telah ada
    function index_put() {
        $id = $this->put('id');
        $data = array(
                    'id'       => $this->put('id'),
                    'nama'          => $this->put('nama'),
                    'nomor'    => $this->put('nomor'));
        $this->db->where('id', $id);
        $update = $this->db->update('telepon', $data);
        if ($update) {
            $this->response($data, 200);
        } else {
            $this->response(array('status' => 'fail', 502));
        }
    }

    //Masukan function selanjutnya disini

14. Untuk mengujinya buka Postman, pilih metode PUT, masukan http://localhost/ci-restserver-master/index.php/kontak pada address bar, klik "Body" pada menu dibawah address bar, pilih x-www-form-urlencoded, masukan key id dan value id yang akan diubah misalkan id=50 diikuti value_id_nama=Agoy, 08546353243, lalu klik "Send"

15. Kemudian, lakukan metode get untuk melihat data baru

<!-- DELETE -->

16. Tambahkan sebuah kode fungsi delete pada kontak.php, silakan cek application/controllers/kontak.php untuk keterangan lebih lanjut.

     //Menghapus salah satu data kontak
    function index_delete() {
        $id = $this->delete('id');
        $this->db->where('id', $id);
        $delete = $this->db->delete('telepon');
        if ($delete) {
            $this->response(array('status' => 'success'), 201);
        } else {
            $this->response(array('status' => 'fail', 502));
        }
    }

17. Untuk mengujinya buka Postman, pilih metode PUT, masukan http://localhost/ci-restserver-master/index.php/kontak pada address bar, klik "Body" pada menu dibawah address bar, pilih x-www-form-urlencoded, masukan key id dan value id yang akan dihapus misalkan id=50, lalu klik "Send"

18. Kemudian, lakukan metode get untuk melihat data baru. Maka data dengan id=50 akan terhapus dalam Rest API Server

19. SELESAI!

<!-- SELESAI -->
