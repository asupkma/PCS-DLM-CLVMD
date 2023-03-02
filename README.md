# PCS-DLM-CLVMD
# yum install pcs dlm lvm2-cluster
# systemctl enable dlm
# systemctl start dlm
# pcs cluster create
# pcs property set no-quorum-policy=freeze
# pcs property set stonith-enabled=false
# lvmconf --enable-halvm
# lvmconf --enable-halvm --services --startstopservices
# systemctl disable lvm2-lvmetad.service   ## RHEL 7
# systemctl disable lvm2-lvmetad.socket    ## RHEL 7
# systemctl stop lvm2-lvmetad.service      ## RHEL 7
# systemctl stop lvm2-lvmetad.socket       ## RHEL 7
# chkconfig lvm2-lvmetad off               ## RHEL 6
# service lvm2-lvmetad stop                ## RHEL 6  
https://www.golinuxcloud.com/configure-ha-lvm-cluster-resource-linux/
https://www-linuxjournal-com.translate.goog/content/high-availability-storage-ha-lvm?_x_tr_sl=en&_x_tr_tl=ru&_x_tr_hl=ru&_x_tr_pto=sc

# systemctl mask lvm2-lvmetad.socket
# Created symlink from /etc/systemd/system/lvm2-lvmetad.socket to /dev/null.
# in lvm.conf locking_type = 3, use_lvmlockd = 0
# pcs resource create dlm ocf:pacemaker:controld op monitor interval=30s on-fail=fence clone interleave=true ordered=true
# pcs resource create clvmd ocf:heartbeat:clvm op monitor interval=30s on-fail-fence clone interleave=true ordered=true
# pcs resource show
# pcs resource create clvmd ocf:heartbeat:clvm op monitor interval=30s on-fail=fence clone interleave=true ordered=true
# pcs constraint order start dlm-clone then clvmd-clone
# pcs constraint colocation add clvmd-clone with dlm-clone
# pvcreate /dev/sdb
# vgcreate -cy data_vg /dev/sdb
# lvcreate -L 50G (-l100%FREE) -n data_lv data_vg

# mkfs.xfs /dev/data_vg/data_lv

# mkfs.ext4 /dev/shared_vg/ha_lv
# lvchange -an shared_vg/ha_lv
# pcs resource create halvm LVM volgrpname=data_vg exclusive=true --group halvmfs
# pcs resource show
# pcs resource create xfsfs Filesystem device="/dev/data_vg/data_lv" directory="/xfs" fstype="xfs" --group halvmfs
# pcs cluster standby node1


