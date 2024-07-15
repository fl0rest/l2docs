.bashrc lets you edit your command propmt to your liking, in our environment you can find 2 seperate .bashrc files.

The first one is in your user directory `~/.bashrc`, and the second one is in `~/dotfiles/.bashrc`
The second one is what gets copied and used on the servers you _me_ into!

You can add the following into your user bashrc to store your ssh key so you don't have to re-enter it every time you _me_ into a server
```
shagent() {
 SSHAGENT=/usr/bin/ssh-agent
 SSHAGENTARGS="-s"
 if [ -z "$SSH_AUTH_SOCK" -a -x "$SSHAGENT" ]; then
 eval `$SSHAGENT $SSHAGENTARGS`
 ssh-add .ssh/nex$(whoami).id_rsa
 trap "kill $SSH_AGENT_PID" 0
 fi
}
```

This is a few examples you can add to your `~/dotfiles/.bashrc` to simplify your on-server workflow
```
#Aliases
alias cdd='sup_cdd'
alias qq='sudo quota -sg $(sup_getusr)'
alias cdlog='sup_cdlogs'
alias go='gouser -n'


#Functions
function slow () {
cuser=$(sup_getusr); limit=$(grep -ish ^pm.max_children /etc/php-fpm.d/$cuser.conf /opt/nexcess/php*u/root/etc/php-fpm.d/$cuser.conf /opt/remi/php*/root/etc/php-fpm.d/$cuser.conf | awk '{print $3}'); k=$(ps uU $cuser | grep pool | grep php-fpm | wc -l); echo -e "\n\n\n============================\n$(date)\n\n == Possible PHP/Apache issues:"; for i in $(ls -1 /opt/nexcess/php*/root/var/log/php-fpm/error.log /var/opt/remi/php*/log/php-fpm/error.log /var/log/php-fpm/error.log /var/log/httpd/error_log 2> /dev/null); do grep --color=never -iHE "$cuser.*max_children|maxclients|scoreboard" $i | tail -n3 ; done; echo -e "\n\n == User quota:\n$(sudo quota -sg $cuser)\n\n == User PHP processes:\n$cuser: $k/$limit threads\n\n\n\n == Server load/uptime:\n$(uptime)\n == Server CPUs: \n$(nproc)\n == Memory info:\n$(free -m | grep -v Swap)\n============================\n";
}

function dkim () {
    sudo -u iworx ~iworx/bin/domainkeys.pex --domain $1;
    wait;
    sudo cat /etc/domainkeys/$1/rsa.public | grep -v '-' | tr -d '\n'; echo
}
function makecsr ()
{
    if [ "$1" ]; then
        success=0;
        echo -e '====================\n   Creating CSR...\nUSE FULL STATE NAME!\n====================';
        openssl genrsa -out ${1}.key 2048 && openssl req -new -key ${1}.key -out ${1}.csr && success=1;
        if [ "$success" == 1 ]; then
            echo -e '====================\ncreated:\n';
            echo ${1}.key;
            echo ${1}.csr;
        else
            echo 'Failure!';
        fi;
    else
        echo 'Domain required!';
    fi
}
iinfo(){
_FILE=$1

if [ -z $_FILE ];
then
        while read line;
        do
                _HITS=$(echo $line | awk '{print $1}');
                _IP=$(echo $line | awk '{print $2}' | cut -d "-" -f1);
                _RAW=$(curl --silent https://ipinfo.io/$_IP);
                _IPC=$(echo $_RAW | jq -r .country )
                _IPR=$(echo $_RAW | jq -r .region )
                _IPO=$(echo $_RAW | jq -r .org )
                echo -e "$_HITS $_IP -- $_IPC - $_IPR / $_IPO";
        done
else
        while read line;
        do
                _HITS=$(echo $line | awk '{print $1}');
                _IP=$(echo $line | awk '{print $2}' | cut -d "-" -f1);
                _RAW=$(curl --silent https://ipinfo.io/$_IP);
                _IPC=$(echo $_RAW | jq -r .country );
                _IPR=$(echo $_RAW | jq -r .region );
                _IPO=$(echo $_RAW | jq -r .org );
                echo -e "$_IP -- $_IPC - $_IPR / $_IPO";
        done < $_FILE
fi
}
speedt(){
read -p "URL: " _url
for i in {0..4}; do
        echo -n "${i}:: "; curl  -o /dev/null -w "Response Code: %{http_code} :: Size: %{size_download} :: Speed: %{speed_download} :: TTFB: %{time_starttransfer} :: Total: %{time_total}\n" -s "$_url";
done
}
```

Explanations of the Functions

`slow` - A function we use to determine primarily how many PHP processes a user is using at the moment, be sure to be in any directory that is owned by a user you want to check (also gives info on quota, server load/uptime, number of cores and memory)

`dkim` - Generate a DKIM key for a site be sure to enter a domain with it -> `dkim somesite.com`

`makecsr` - Generate a CSR for a customer, usually they can make it themselves in the portal, but you might get a request for it, you will need some information for this, search waypoint/help articles

`iinfo` - Get information about IPs, you can pipe into it or specify a file for it to look through

`speedt` - A reaaaally basic page load test

 -- With love, Dougie <3
