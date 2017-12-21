Linux


# Start with virtual box

To play with linux virtual machine, you can choose to use VMplayer or VirtualBox.
VirtualBox from Oracle has a better licence.

You can change VirtualBox [language](http://ccm.net/faq/40865-virtualbox-how-to-change-the-language-settings)


You can download your Linux image and verify the download [checksum](https://www.lifewire.com/validate-md5-checksum-file-4037391), make sure that your download is perfect.
```
certutil -hashfile bodhi-4.1.0-64.iso MD5 
```
output should be the same as your MD5 file, if the values don't match then the file is not valid and you should download it again.


To share your host harddisk with guest linux [virtual machine](https://www.youtube.com/watch?v=h8cA3pIAeZQ).
https://www.htpcbeginner.com/mount-virtualbox-shared-folder-on-ubuntu-linux/


# Linux file read
[cat vs grep vs awk vs sed comparison](https://askubuntu.com/questions/564944/cat-vs-grep-vs-awk-command-get-the-file-content-which-one-is-more-efficient-and)
```
# show your centOS version
$ cat /etc/centos-release
$ awk 1  /etc/centos-release

$ time for i in {1..1000}; do cat temp.txt; done
$ time for i in {1..1000}; do grep "$" temp.txt; done
$ time for i in {1..1000}; do awk "/$/" temp.txt; done
```
