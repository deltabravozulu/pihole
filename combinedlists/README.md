Here are a bunch of combined hosts files I found in various repositories, some long gone.
If you happen to have a bunch of hosts files to combine and dedupe, here's the clumsy way I did it.

First, install rexreplace, a nice regex tool:
`npm install -g rexreplace`

cat a list of files into single one, e.g.: 

`cat *.txt >> hosts.txt`

change titlecase, sort it, remove duplicates, get rid of IPs and comments:


`file=hosts.txt`

```cat $file | tr "[:upper:]" "[:lower:]" | sort | uniq | rexreplace "\t+" " " | rexreplace "^(\d+\.\d+\.\d+\.)?\d+\s+" "" | rexreplace "\#.*" "" | rexreplace ".*\:.*\n" "" | rexreplace "^([a-f][a-f]\d\d)?:+\d?[\%\s](ip\d|lo\d)[\s\-](localhost|mcastprefix|localnet|allnodes|allrouters|allhosts|loopback)" "" | rexreplace "^\<.*\n" "" | rexreplace "/.*" "" | rexreplace "^\d+\.\d+\.\d+\.\d+$" "" | rexreplace "\s+\n+" "\n" | rexreplace ".* .*" "" | rexreplace ".*:.*" "" | rexreplace "^[\w\-\_]+$" "" | rexreplace "^[^.]*$" "" | rexreplace "^(www\.)+" "" | rexreplace "\n+" "\n" | sort | uniq >>newhosts.txt```
