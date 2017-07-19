# SpeedTest-Average
Using https://github.com/rsvp/speedtest-linux this script holds a running average of ping, download, and upload


    #clear old results
    rm -rf results.txt

    while true;
        do
        echo "Running speedtest now. Please wait..."
        ./speedtest | sed -e 's|,||g' >> results.txt
        #awk '{s+=$3}END{print "ave:",s/NR}' RS=" " results.txt
        ping=$(cat results.txt | awk '{print $3}' | jq -s 'add/length')
        download=$(cat results.txt | awk '{print $4}' | jq -s 'add/length')
        upload=$(cat results.txt | awk '{print $5}' | jq -s 'add/length')
        counter=$((counter+1))


        runnumber=$(wc -l results.txt | awk '{print $1}')
        echo "Current - " $runnumber " run number "
        currentping=$(cat results.txt | tail -1 | awk '{print $3}')
        currentdl=$(cat results.txt | tail -1 | awk '{print $4}')
        currentul=$(cat results.txt | tail -1 | awk '{print $5}')
        
        echo "Ping: " $currentping 
        echo "Download: " $currentdl 
        echo "Upload: " $currentul 

        echo "----------------------"

        echo "Average after - " $runnumber " runs"
        echo "Ping: " $ping
        echo "Download: " $download
        echo "Upload: " $upload 
        
        echo "----------------------"

        sleep 1
        
done;
