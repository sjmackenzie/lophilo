# Lophilo Benchmarks

## Storage performance

### NFS root

	root@lophilo1# dd if=/dev/zero of=~/test bs=1048576 count=16
	16+0 records in
	16+0 records out
	16777216 bytes (17 MB) copied, 1.76349 s, 9.5 MB/s


### fat32 partition

	root@lophilo1# dd if=/dev/zero of=/media/boot/test bs=1048576 count=16
	16+0 records in
	16+0 records out
	16777216 bytes (17 MB) copied, 2.816 s, 6.0 MB/s

### ext4 partition

	root@lophilo1# dd if=/dev/zero of=/media/os/test bs=1048576 count=16
	16+0 records in
	16+0 records out
	16777216 bytes (17 MB) copied, 0.379735 s, 44.2 MB/s
