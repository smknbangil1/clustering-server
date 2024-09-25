# clustering-server
## clustering server ujian sekolah menggunakan moodle 4 node
### Topologi 
*node1* moodle1 (nginx, php, mariadb)

*node2* moodle1 (nginx, php, mariadb)

*node3* moodle1 (nginx, php, mariadb)

*node4* haproxy 

### Urutan kerja
#### beresin cluster database dulu
1. Install `mariadb-server` `mariadb-client` `galera` pada node1, node2, node3
2. Konfigurasi cluster database dan ujicoba singkron database
3. install juga `nginx` dan `php` test sampai bisa buka web bawa'an nginx di browser
4. konfigurasi nginx, php dan optimasi belakangan, utamakan fongsional dulu
#### membuat cluster storage
folder direktori /var/www/html/moodle/ ini digunakan untuk menyimpan kode web html, css, php
folder direktori moodledata/ ini untuk konten gambar dan audio-video
pada direktori `moodledata/` saya menggunakan glusterfs bisa berjalan lancar, katanya sih gabungan gluster + nsf ganesha bisa lebih cepat utk proses singkronisasi file dan folder daripada glusterfs murni. 

Dan pada direktori /var/www/html/moodle/ masih disimpan mandiri pada masing-masing node dan singkron manual dengan rsync karena direktori ini paling sibuk diakses, saya belum menemukan cara clustering storage yang paling cepat (setidaknya bisa mendekatai kecepatan i/o ssd tanpa cluster)
#### install nginx dan php-fpm
install seperti biasa aja, konfigurasi vhost cukup arahkan moodledata/ pada mounting volume glusterfs, dan /var/www/html/moodle/ pada folder masing-masing node
#### install redis server
