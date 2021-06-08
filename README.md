# amblist_authorised

Amblist map of every MVS load library that runs authorised.




Generated with the following:

tsocmd "ex 'jake.rexx(listapf)'" > listapf

while IFS= read -r line; do
    echo "Text read from file: $line"
    echo " LISTLOAD OUTPUT=MAP" | amblist "//'$line'"  > a$line
done < listapf        


while IFS= read -r line; do
    echo "Text read from file: $line"
    grep -C 25  -i "APFCODE:           00000001" a$line  |  sed -n -e 's/^.*MEMBER NAME: //p' | awk '{print $1}' > authorised_$line 
done < listapf      


while IFS= read -r line; do
    echo "Text read from file: $line"
    while IFS= read -r line2; do
      echo "Text read from file2: $line2"
      echo " LISTLOAD OUTPUT=MAP,MEMBER=$line2" | amblist "//'$line'"  > am$line$line2
    done < authorised_$line     
done < listapf   
