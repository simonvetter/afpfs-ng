all: prepare fuse_anon fuse_auth

fuse_anon: 
	mkdir -p mnt
	mount_afp afp://delorean.local/DeLorean `pwd`/mnt
	ls -al mnt/ | grep com.apple.timemachine.supported >/dev/null 
	fusermount -u `pwd`/mnt
	@echo 'fuse_anon test passed.'

fuse_auth:
	mkdir -p mnt
	mount_afp afp://test:test_pwd@delorean.local/test `pwd`/mnt/
	echo 'You should read this back' >mnt/sample.txt
	sleep 1
	fusermount -u `pwd`/mnt
	sleep 1
	mount_afp afp://test:test_pwd@delorean.local/test `pwd`/mnt
	grep -E '^You should read this back$$' mnt/sample.txt >/dev/null
	wc -l mnt/sample.txt | grep 1 >/dev/null
	rm mnt/sample.txt
	sleep 1
	fusermount -u `pwd`/mnt
	@echo 'fuse_auth test passed.'

prepare:
	fusermount -u `pwd`/mnt >/dev/null || true
	sleep 1
	killall afpfsd || true
