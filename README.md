### OSD(S) prediction using linear regression
Osd(s) or Object Storage Daemon(s) is storage daemon for the Ceph distributed file system. It is responsible for storing objects on a local file system and providing access to them over the network.

### Purpose
Because it's was `Storage` so osd(s) will be full when the data already big, from ceph its self will be give a warning if some osd(s) was [nearfull](https://docs.ceph.com/en/latest/rados/troubleshooting/troubleshooting-osd/#no-free-drive-space) and if ignore the warning and the data still increasing ceph will trigger another warning [full osd(s)](https://www.suse.com/support/kb/doc/?id=000019724) and if `full osd(s)` already triggering the ceph cluster will be read-only until resizing osd(s) and error `full osd(s)` was gone.


And here the problem, if we want to resize the osd(s) when `nearfull` it will activate the recover and recover its self will make degradation IOPS (especially if your osds was hdd) and the second problem is data keep increasing meanwhile some osd(s) was dying and the last problem we can't do resize ASAP because adding new osds will take some times (ordering new hardware) or we can't just resize the nearfull osd(s) when production time (in my case, I only have 8 hours to do resize meanwhile the data keep increasing every day).


So because of those problem i was thingking, can i just predict the usage of osd(s) so before the nearfull event we already prepare the resize or adding new osds.

### Dataset
I only have 2 dataset ssd&hdd for the hdd the data was very messy because some hardware problem and another problem 

##### Why use linear regression?
that only algorithm i know and understand very well,if you have some suggestions I'm very welcome  


##### Note
I'm don't have data science background so the code was messy,just prepare your eye