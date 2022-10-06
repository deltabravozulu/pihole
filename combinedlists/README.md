Here are a bunch of combined hosts files I found in various repositories.
Ultimately, it's just a lot of block lists concatenated, cleaned, and deduped. It consists of 4102660 sites split into 4 files, 1025665 lines each along with the lists that are no longer online.

1. [bigcombined_1_0000001-1025665.txt](https://raw.githubusercontent.com/deltabravozulu/pihole/main/combinedlists/bigcombined_1_0000001-1025665.txt)
2. [bigcombined_2_1025666-2051330.txt](https://raw.githubusercontent.com/deltabravozulu/pihole/main/combinedlists/bigcombined_2_1025666-2051330.txt)
3. [bigcombined_3_2051331-3076995.txt](https://raw.githubusercontent.com/deltabravozulu/pihole/main/combinedlists/bigcombined_3_2051331-3076995.txt)
4. [bigcombined_4_3076995-4102660.txt](https://raw.githubusercontent.com/deltabravozulu/pihole/main/combinedlists/bigcombined_4_3076995-4102660.txt)
5. [bigcombo_404.txt](https://raw.githubusercontent.com/deltabravozulu/pihole/main/combinedlists/bigcombo_404.txt)

If you happen to have a bunch of hosts files to combine and dedupe, here's the clumsy way I did it.

First, install rexreplace, a nice regex tool:
`npm install -g rexreplace`

cat a list of files into single one, e.g.: 

`cat *.txt >> hosts.txt`

change titlecase, sort it, remove duplicates, get rid of IPs and comments:


`file=hosts.txt`

```cat $file | tr "[:upper:]" "[:lower:]" | sort | uniq | rexreplace "\t+" " " | rexreplace "^(\d+\.\d+\.\d+\.)?\d+\s+" "" | rexreplace "\#.*" "" | rexreplace ".*\:.*\n" "" | rexreplace "^([a-f][a-f]\d\d)?:+\d?[\%\s](ip\d|lo\d)[\s\-](localhost|mcastprefix|localnet|allnodes|allrouters|allhosts|loopback)" "" | rexreplace "^\<.*\n" "" | rexreplace "/.*" "" | rexreplace "^\d+\.\d+\.\d+\.\d+$" "" | rexreplace "\s+\n+" "\n" | rexreplace ".* .*" "" | rexreplace ".*:.*" "" | rexreplace "^[\w\-\_]+$" "" | rexreplace "^[^.]*$" "" | rexreplace "^(www\.)+" "" | rexreplace "\n+" "\n" | sort | uniq >>newhosts.txt```
