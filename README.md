_imported from http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/tx.htm_

# Tx: Succinct Trie Data structure

## Introduction

**Tx** is a library for a compact trie data structure. Tx requires 1/4 - 1/10 of the memory usage compared to the previous implementations, and can therefore handle quite a large number of keys (e.g. 1 billion) efficiently. A trie data structure supports exact matching and common prefix matching, which are used for natural language processing etc. Tx uses Level-Order Unary Degree Sequence (LOUDS) for trie representation.

## Download
**Tx** is free software; you can redistribute it and/or modify it under the terms of the new BSD License.

*   tx-0.13.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.13.tar.gz)

Archives:

*   tx-0.12.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.12.tar.gz)
*   tx-0.11.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.11.tar.gz)
*   tx-0.10.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.10.tar.gz)
*   tx-0.09.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.09.tar.gz)
*   tx-0.08.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.08.tar.gz)
*   tx-0.07.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.07.tar.gz)
*   tx-0.06.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.06.tar.gz)
*   tx-0.05.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.05.tar.gz)
*   tx-0.04.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-0.04.tar.gz)
*   tx-0.03.tar.gz: [HTTP](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/software/tx-003.tar.gz) (obsolete)

## News

*   **2009-04-13**: Tx 0.13  

    *   0.13 Release
    *   Fix memory leak bugs at resize()
*   **2008-06-03**: Tx 0.12  

    *   0.12 Release
    *   Fix configure.ac (Now support Ubuntu)
*   **2008-05-28**: Tx 0.11  

    *   0.11 Release
    *   Fix bugs when # keys is 0.
*   **2008-04-06**: Tx 0.10  

    *   0.10 Release
    *   Change the default value of limit from 0 to 0xffffffff.
    *   Fix bugs (release of resourse at mmap)
*   **2008-03-23**: Tx 0.09  

    *   0.09 Release
    *   Add reverseLookup() function.
    *   Add sample programs: s2sbuild s2ssearch
*   **2008-02-27**: Tx 0.08  

    *   0.08 Release
    *   Add setArray() function.
    *   Add different versions of commonPrefixSearch(), and predictiveSearch() functions.
    *   Add txsearch_mmap sample program.
*   **2008-02-13**: Tx 0.07  

    *   0.07 Release
    *   Add commonPrefixSearch(), predictiveSearch(), getKeyNum() functions.
*   **2008-01-23**: Tx 0.06  

    *   0.06 Release
    *   Fix txbuild bug (It works correctly even if a keyword has spaces or tabs)
*   **2007-12-24**: Tx 0.05  

    *   0.05 Release
    *   Remove stderr output. Add getResultLog(), getErrorLog() functions to get log messages.
*   **2007-10-06**: Tx 0.04  

    *   0.04 Release
    *   Change expansionSearch interface and return values. Return all partially matched keys
*   **2007-05-27**: Tx 0.03  

    *   0.03 Release
    *   Change prefixSearch interface. Return unique keyword ID and prefix match length.
*   **2007-05-11**: Add performance results about exact matching  

*   **2007-05-02**: Tx 0.02  

    *   0.02 Release
    *   Fix prefixSearch buf
    *   Add expandSearch
*   **2007-03-03**: Tx 0.01  

    *   Initial Release

## Usage

How to make: configure; make; make install  
How to use library: configure; make; sudo make install You add `"#include <tx.hpp>` to your program, and put "-ltx" as a compiler option. You can change default install path by using --prefix option of configure script. Try --help option for finding out other options.

Tx reads a file containing a keyword per line, and then builds a dictionary and stores it in the index. When you use Tx, you need to read the dictionary from the index and then call prefixMatch.

See the example in txbuild.cpp and txsearch.cpp.

## Sample Program

### txbuild

<pre>% ./txbuild wordlist index 
</pre>

Build a Tx dictionary from a file (wordlist) which contains a keyword per a line, and then store the dictionary to index.

### txsearch

<pre>% ./txsearch index
</pre>

Read the dictionary from index, and then perform common prefix search in interactive mode.

### txsearch_mmap

<pre>% ./txsearch_mmap index
</pre>

Read the dictionary from index as txsearch, but keep the index in mmap. Then perform common prefix search in interactive mode.

### s2sbuild

<pre>% ./s2sbuild word2wordlist index
</pre>

Build a mapping dictionary from a word to a word. Input file (word2wordlist) containts keyword pairs per a line. Each line contains keyword pairs, which are separated by tab. This build index.1, index.2, and index.12\.

### s2ssearch

<pre>% ./s2ssearch index
Read the dictionary from index, and then perform common prefix search in interactive mode. It shows all mapped words against all substring of a query.
</pre>

### Example

<pre>% head wordlist
Abraham
Abrahamn
Abramsky,
Absalom
Abson,
...
% ../txbuild wordlist wordlist.tx
% ../txsearch wordlist.tx
keyNum:682496 nodeNum:4480509
>abraham
prefixSearch id:156477 len:7
expansionSearch 9
a
ab
abr
abra
abraham
abrahams
abrahamic
abrahamian
abrahamitische
commonPrefixSearch 5
a (id=0)
ab (id=27)
abr (id=734)
abra (id=14775)
abraham (id=156477)
predictiveSearch 5
abraham (id=156477)
abrahams (id=183069)
abrahamic (id=206905)
abrahamian (id=295025)
abrahamitische (id=413017)

(elemNum is the number of keys, and nodeNum is the number of node in the dictionary)
</pre>

## Interface

`int tx_tool::tx::build(vector<string>& wordList, const char* fileName) const`  
Build a Tx dictionary from wordList and then store it in the file specified by fileName. Note that you need to call read if you use the dictionary immediately.  
wordList is the array of keys to store. These keys are not necessarily sorted. Duplicated keys are removed at the building time  
fileName is the name of file to store the index. If the file is already exist, this will be replaced．

`int tx_tool::tx::read(const char* fileName)`  
Read a Tx index from fileName. If an error occurs, return -1.

`tx_tool::uint tx_tool::tx::prefixSearch(const char* str, const size_t len, size_t& retLen) const;`  
Search the keyword that has the longest prefix match to str and return the key ID if exist. Return tx_tool::NOTFOUND if such keywords are not found. retLen will have prefix match Length.  
A key ID is the unique number, assgined from 0 to (# keys - 1).

`tx_tool::uint tx_tool::tx::expandSearch(const char* str, const size_t len, vector<string>& ret, const size_t limit=TX_LIMIT_DEFAULT) const;`  
Store all partially matched keys against str in ret. Return # keys in ret. If limit is specified, return at most limit keys.

`uint commonPrefixSearch(const char* str, const size_t len, std::vector<std::string>& ret, std::vector<uint>& retID, const uint limit = TX_LIMIT_DEFAULT) const;`  
Store all keywords in ret, which match to the prefix of str and store these key IDs in retID. Return # keys in ret. If limit is specified, return at most limit keys.

`uint commonPrefixSearch(const char* str, const size_t len, std::vector<uint>& retLen, std::vector<uint>& retID, const uint limit = TX_LIMIT_DEFAULT) const;`  
Store all length of keywords in retLen, which match to the prefix of str and store these key IDs in retID. Return # keys in ret. If limit is specified, return at most limit keys.

`uint predictiveSearch(const char* str, const size_t len, std::vector<std::string>& ret, std::vector<uint>& retID, const uint limit = TX_LIMIT_DEFAULT) const;`  
Store all keywords in ret, whose prefix match to str and store these key IDs in retID. Return # keys in ret. If limit is specified, return at most limit keys.

`uint predictiveSearch(const char* str, const size_t len, std::vector<uint>& retLen, std::vector<uint>& retID, const uint limit = TX_LIMIT_DEFAULT) const;`  
Store all length of keywords in retLen, whose prefix match to str and store these key IDs in retID. Return # keys in ret. If limit is specified, return at most limit keys.

`tx_tool::uint tx_tool::tx:reverseLookup(const tx_tool::uint id, std::string& ret) const;`  
Store key in ret from id. Return the length of key. If id is invalid, this returns 0.

`uint predictiveSearch(const char* str, const size_t len, std::vector<uint>& retLen, std::vector<uint>& retID, const uint limit = 0) const;`  
Store all length of keywords in retLen, whose prefix match to str and store these key IDs in retID. Return # keys in ret. If limit is specified, return at most limit keys.

`string tx_tool::tx::getResultLog();`  
Return operation log in build/read functions.

`string tx_tool::tx::getErrorLog();`  
Retrun error log in build/read function.

`string tx_tool::tx::getKeyNum();`  
Return the number of registerd keys.

`string tx_tool::tx::setArray(void* ptr, size_t readSize);`  
Read a Tx index from ptr as in read(). You can use this if the index is managed by mmap. Please see the example in txsearch_mmap.cpp.

TX\_LIMIT\_DEFAULT is 0xffffffff (the max value for unsinged 32bit integer).

## Perfomance evaluatoin

### Index Size and Construction time

Input data

*   [Web 1T 5-gram Version 1](http://www.ldc.upenn.edu/Catalog/CatalogEntry.jsp?catalogId=LDC2006T13)
*   Use 1gms list as a wordlist (removing frequency information)
*   Number of types of keyword: 13,588,391
*   Total input size (concatenating all keywords): 112,349,445 bytes
*   Number of nodes in trie (including leaves): 34,722,820


||Size of index(ratio against input size)|Result of time command|
|---|---|---|
|tx (txbuild)|52,626,805 (46.8%)|30.581u 1.137s 3:50.95 13.7%|
|darts (mkdarts)|406,358,432 (361.7%)|16.688u 1.785s 8:13.52 3.7%|

### Search Perforamnce

Input data

*   Keywords extracted from [Wikipedia Japanese](http://ja.wikipedia.org/wiki/Wikipedia:%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89)
*   Number of types of keyword: 682,496
*   Total input size (concatenating all keywords): 10,640,194 bytes
*   Number of nodes in trie: 4,531,308
*   We measured the average time of exact machings of all entries in index (only exact matching)

||Time per a query (micro-second) *1|
|---|---|
|tx 0.02|10.8|
|`std::map<std::string,int>`|4.06|
|darts 0.31|1.05|
|`std::tr1::unordered_map<std::string,int>`|0.63|

*1 This is the result of clock(). The result of gettimeofday is similar one.

## TODO

*   Refine bad English (Please comment.)
*   Interfaces
*   Performance comparison
*   Write Tech details
*   Speedup and reduce memory usage
*   Application examples

## Others

Tx is named after a train name in which I programmed this software.

## Reference

*   [Bep](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/bep-j.htm): Associative Arrays using Minimal Perfect Hash Functions. Fast and small.
*   [tx-ruby](http://gimite.net/en/index.php?tx-ruby) :Tx Ruby binding．
*   [Text-Tx](http://coderepos.org/share/browser/lang/perl/Text-Tx/): Tx Perl binding.
*   [Darts (Japanese)](http://www.chasen.org/~taku/software/darts/): Trie implementation by using Double Array
*   G. Jacobson. Space-efficient static trees and graphs. In Proc. 30th FOCS, 549-554
*   O. Delpratt, N. Rahman, R. Raman. Engineering the LOUDS Succinct Tree Representation. ([pdf](http://www.mcs.le.ac.uk/~ond1/XMLcomp/confersWEA06_LOUDS.pdf))  
    Proceedings of WEA 2006, LNCS 4007, p. 134-145\.

## Contact

Comments or Bug report  
[Daisuke Okanohara](http://www-tsujii.is.s.u-tokyo.ac.jp/~hillbig/atlab.html) (hillbig at is.s.u-tokyo.ac.jp) (Change "at" to @）
