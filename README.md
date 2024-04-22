#!/bin/bash

function ctrl_c() {
    echo -e "\nExiting..."
    exit 0
}

# Function to monitor log file for new entries
function monitor_log() {
    echo "Monitoring log file: $log_file"
    tail -n 0 -f "$log_file" | while read -r line; do
        analyze_log_entry "$line"
    done
}

# Function to analyze log entry
function analyze_log_entry() {
    # Add your analysis logic here
    error_pattern="error"
    if [[ $1 =~ $error_pattern ]]; then
        echo "Error found: $1"
    fi
}

# Main function
function main() {
    if [ "$#" -ne 1 ]; then
        echo "Usage: $0 <log_file>"
        exit 1
    fi

    log_file="$1"

    trap ctrl_c INT  # Register signal handler for Ctrl+C

    monitor_log "$log_file"
}

main "$@"
