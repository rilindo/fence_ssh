# fence_ssh
Fencing agent that uses SSH, should only be used for testing.

The branch alternate_version contains a version of the agent that works using the nodename parameter instead (read the readme on that branch).

## How to Use
Place in `/usr/sbin` with execute permissions. Then you should be able to see it by running:

```
[root@myhostname mnt]# pcs stonith list
<snip>

fence_apc_snmp - Fence agent for APC, Tripplite PDU over SNMP
fence_ssh - Basic fencing agent that uses SSH
fence_virt - Fence agent for virtual machines
<snip>
```

## Create Stonith Resources

You can see usage with either `fence_ssh --help` or by running `pcs stonith describe fence_ssh`

Example usage with PCS:

```
pcs stonith create stonith-one fence_ssh user=centos hostname=host1 password=Pa55w0rd sudo=true pcmk_host_list="host1"
pcs stonith create stonith-two fence_ssh user=centos hostname=host2 password=Pa55w0rd sudo=true pcmk_host_list="host2"
pcs constraint location add stonith-one-host-one stonith-one host1 INFINITY
pcs constraint location add stonith-two-host-two stonith-two host2 INFINITY
```

_Note: Ensure that pcmk_host_list is defined so that Pacemaker knows which resource can fence which node_
