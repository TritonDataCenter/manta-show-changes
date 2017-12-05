# manta-show-changes

`manta-show-changes` is a quick-and-dirty tool that attempts to report a concise
list of changes integrated into "master" since a particular
[Manta](https://github.com/joyent/manta) component version.


## Prerequisites

You must have clones of the various top-level Manta repositories.  This tool
currently assumes they're available in `$HOME/work/reference/manta`.

An alternate implementation could potentially use the GitHub API.


## Synopsis

First, see the prerequisites above.

Next, clone and build this repository:

    $ git clone https://github.com/joyent/manta-show-changes
    $ cd manta-show-changes
    $ make

Make sure each of your clones is clean first:

    $ for repo in $HOME/work/reference/manta/*; do (cd $repo; git status); done

If the output looks okay, then make sure they're all up to date:

    $ for repo in $HOME/work/reference/manta/*; do (cd $repo; git pull); done

Generate a list of unique service names and versions from the Manta deployment
you want to run this on.  If this is a multi-datacenter deployment, run the
`manta-adm` part in each DC, concatenate them all, and then run the `sort -u`
part.

    [root@headnode (emy-10) ~]# manta-adm show -s -o service,version | 
        sort -u > service_versions.txt

The output will look something like this:

    authcache        master-20170919T233646Z-g75ac3ca          
    electric-moray   master-20171103T213507Z-gbfd1aa5          
    jobpuller        master-20170920T001026Z-g95b5f73          
    jobsupervisor    master-20170919T234226Z-gb8af6a8          
    loadbalancer     master-20170919T235445Z-g66b2bf1          
    madtom           master-20170919T232545Z-g74f7418          
    marlin           master/16.1.0                             
    marlin-dashboard master-20170919T234202Z-g9cae708          
    medusa           master-20170919T234916Z-g7b01c12          
    moray            master-20171009T235739Z-g89d1860          
    nameservice      master-20170919T232319Z-gb136622          
    ops              master-20170919T235820Z-g94b1e89          
    postgres         master-20171009T223219Z-g2bead11          
    SERVICE          VERSION                                   
    storage          master-20170919T233349Z-g838ab1b          
    webapi           master-20171009T215612Z-ga612da7          

Copy the above concatenated, deduplicated file to the system with all the
clones in $HOME/work/reference/manta.

Finally, you're ready to run this tool:

    $ ./bin/manta-show-changes < service_versions.txt
    authcache: currently at master-20170919T233646Z-g75ac3ca
            MANTA-3508 invalid user added to "operators" group, mahi sadness ensues
    
    electric-moray: currently at master-20171103T213507Z-gbfd1aa5
            MORAY-445 Update Electric Moray to node-fast v2
    
    jobpuller: currently at master-20170920T001026Z-g95b5f73
    
    jobsupervisor: currently at master-20170919T234226Z-gb8af6a8
    
    loadbalancer: currently at master-20170919T235445Z-g66b2bf1
    
    madtom: currently at master-20170919T232545Z-g74f7418
    
    marlin-dashboard: currently at master-20170919T234202Z-g9cae708
    
    medusa: currently at master-20170919T234916Z-g7b01c12
    
    moray: currently at master-20171009T235739Z-g89d1860
            MORAY-436 Port arguments need better validation
            MORAY-438 Documentation lists NoDatabaseError instead of NoDatabasePeersError
            MORAY-304 Remove numWorkers from all documentation and configs/templates
            MORAY-443 Clean shutdown broken by MORAY-421
            MORAY-428 Make it safer to use reindexing buckets
            MORAY-447 strange latency data point in reported moray metrics
            MANTA-3475 muskie should report 503 errors when moray hits its max queue length
            MANTA-3503 moray pgclient query function creates its own request id
    
    nameservice: currently at master-20170919T232319Z-gb136622
    
    ops: currently at master-20170919T235820Z-g94b1e89
    
    postgres: currently at master-20171009T223219Z-g2bead11
            MANATEE-365 pg_dump backup does not work with PostgreSQL 9.6.3
    
    storage: currently at master-20170919T233349Z-g838ab1b
    
    webapi: currently at master-20171009T215612Z-ga612da7
            MANTA-3350 Add query parameter to muskie that allows operators to delete upload directories and parts
            MANTA-3205 occasional errors about picker having no connections
            MANTA-3430 could add muskie metrics for throughput
            MANTA-3475 muskie should report 503 errors when moray hits its max queue length
            MANTA-2409 Muskie pagination subject to Moray filter injection
            MANTA-2803 Cannot mrmdir or mls in manta folder with parentheses in name
            MANTA-2947 muskie storage utilization threshold must be configurable
