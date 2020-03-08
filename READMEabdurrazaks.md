Pembuatan RestApi server diperlukan:
1.Webserver seperti Xampp, Wampp, atau lainnya.
2.Codeigniter dan library REST server yang diperlukan (contoh : https://github.com/ardisaurus/ci-restserver.)
3.extract Codeigniter dan library REST server yang telah didownload dan pindah ke htdocs pada direktori xampp lalu rename folder Codeigniter dan library REST server menjadi rest_ci atau rename dengan nama folder yang ingin dibuat.
4.Untuk melihat instalasi berhasil atau tidak, masukkan  http://127.0.0.1/rest_ci/index.php/rest_server pada browser.
5.Melakukan konfigurasi database.
Pastikan untuk mendownload dan menginstall aplikasi postman di https://www.postman.com
    Pada aplikasi postman terdapat beberapa metode yaitu :
    1.GET untuk membaca data dari tabel telepon pada database kontak.
    2.POST untuk mengirimkan data baru dari client ke server REST API.
    3.PUT memperbarui data yang telah ada di server REST API.
    4.DELETE untuk menghapus data yang telah ada di server REST API.

Pertama Buat file baru bernama kontak.php di dalam controllers lalu masukkan code dibawah

<?php

defined('BASEPATH') OR exit('No direct script access allowed');

require APPPATH . '/libraries/REST_Controller.php';
use Restserver\Libraries\REST_Controller;

class Kontak extends REST_Controller {

    function __construct($config = 'rest') {
        parent::__construct($config);
        $this->load->database();
    }

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

}
?>

setelah code diatas di save buka aplikasi postman dengan alamat http://127.0.0.1/rest_ci/index.php/kontak