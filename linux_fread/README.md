### No. 1: Integrate the linux command with the "data.table" package to read the large files faster.

---

One of the best R packages to work with large files is "data.table" package. It's super-fast, memory-efficient when you are working with large files. [Here is the GitHub page](https://github.com/Rdatatable/data.table).

In R, sometimes you need to work only with specific columns and apply the filter based on string or numbers, so you don't need all the data from the data-set. You could do it with "data.table" package to filter out the columns, but there is another efficient way to do it. 

For example, let's say you have data-file like this:

| seqnames | start | strand | context | c.meth | counts.total | posteriorMax | status | rc     | tr  |
|----------|-------|--------|---------|--------|--------------|--------------|--------|--------|-----|
| 1        | 1     | +      | CHH     | 0      | 0            | 0.7596       | I      | 0.0887 | CCC |
| 1        | 80    | -      | CHH     | 1      | 12           | 0.9261       | I      | 0.0462 | CAT |
| 1        | 107   | +      | CHH     | 2      | 14           | 0.8459       | M      | 0.2432 | CCC |
| 1        | 108   | +      | CHG     | 11     | 14           | 0.9999       | M      | 0.5674 | CCG |
| 1        | 109   | +      | CG      | 13     | 14           | 0.9999       | M      | 0.8511 | CGA |
| 1        | 110   | -      | CG      | 12     | 12           | 0.9999       | M      | 0.8511 | CGG |
| 1        | 114   | +      | CHG     | 0      | 12           | 0.8809       | I      | 0.1032 | CCG |
| 1        | 115   | +      | CG      | 10     | 11           | 0.9999       | M      | 0.8511 | CGG |
| 1        | 116   | -      | CG      | 12     | 12           | 0.9999       | M      | 0.8511 | CGG |

I need to work with this table that includes only "CHH" in the 'context' column. 

Imagine the size of this data-file is about 6 GB, which means it will take computational resources to import in R and apply filters to the data.


So, here the power of Linux and "data.table" is coming. We could apply the grep command to filter out the text before reading by data.table
Here is the code:

```R
testFile<-"test.txt"
command <- sprintf(paste0("grep --text -w CHH %s"),
  paste(testFile, collapse = ""))

tmp <- fread(cmd = command)
```

As you could see, I write a small function that uses the "grep" to filter the data based on 'CHH' then in the 'fread' I'm calling that function.

Of course, instead of the "grep" command, you could use standard Linux commands to apply various filters on your data-file then import into R. It will save a lot of your memory and reading time when you are working with large data-sets.

