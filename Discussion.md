to solve , we need to efficiently extract logs from a very large data set.
approach:
binary search for efficient start position: since log file is sorted by timestamps we can use binary search to quickly locate the first occurence of the target date. this avoids reading the entire file and significantly reduces the search time.
once the start position of the target date is found we sequentially read the log entries until date changes. this ensures we capture all logs for the specified date without unnecessary memory usage.
