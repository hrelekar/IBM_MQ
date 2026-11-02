# Script for clearing Queues which are having IPPROCS & OPPROCS 

Go to below path

cd /opt/mqm/bin


      #!/bin/bash
      QMGR="<QMGR_NAME>"
      
      # Extract queues with CURDEPTH > 0
      QUEUES=$(echo "dis ql(*) where(curdepth gt 0) ipprocs opprocs " |
               runmqsc "$QMGR" |
               awk -F'[()]' '/^[[:space:]]*QUEUE\(/ {print $2}')
      
      # Process each queue
      for QUEUE in $QUEUES
      do
          echo "========================================"
          echo "Clearing Queue: $QUEUE"
          echo "========================================"
      
          ./dmpmqmsg -m "$QMGR" -I "$QUEUE"
      
          echo "Completed: $QUEUE"
      done  
      echo "All matching queues processed."
