### The challenge:


```
ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU
```


---

### THE SOLUTION:

Was bored so I came up with this disgusting solution:
![image](https://github.com/user-attachments/assets/371241aa-9269-4892-a5ed-79213d9fa16d)

```bash
D=$(which curl)
P='https://'
A='digwebinterface.com'
C='?hostnames='
F='&type='
H='&showcommand='
I='&short='
N='&ns='
Z='&useresolver='
X='&nameservers='
DATA="ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU"
TEMP=$(mktemp)
echo ${DATA} | base64 -d > ${TEMP}
DOMAIN=$(grep -oP "[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&//=]*)" ${TEMP})
DATA=$(cat ${TEMP})
echo "Data contains: ${DATA} with domain: ${DOMAIN}"
COUNTER=0
for (( i=0; i<$(cat ${TEMP} | wc -c); i++)) {
    if [[ ${DATA:$i:1} =~ '?' ]];
    then
        COUNTER=$((COUNTER + 1))
    fi
}
echo "Magic counter: ${COUNTER}"
END=$(printf "%0.s9" $(seq 1 ${COUNTER}))
START=0
echo "Starting at: ${START}, Ending at: ${END}"
for i in $(seq ${START} ${END});
do
    G=$(printf "egg%04d%s" ${i} ${DOMAIN})
    echo "Trying ${G}"
    ${D} -sk "${P}${A}/${C}${G}${F}TXT${H}on${I}on${N}resolver${Z}208.67.222.222${X}" | grep -ioP '^&.*found.*'
done
rm -rf ${TEMP}
kill -9 $$
```
