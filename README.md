# Checklist Offline-Online PIR adapted for Mobile Contact Discovery

This code was adapted to meet the requirements for mobile-contact-discovery. 


*This code accompanies the [Checklist paper](https://eprint.iacr.org/2021/345.pdf) by [Dmitry Kogan](https://cs.stanford.edu/~dkogan/) and [Henry Corrigan-Gibbs](https://people.csail.mit.edu/henrycg/).*

Checklist is a system for privacy-preserving blocklist lookups. That is: a pair of servers holds a set *B* of blocklisted strings, a client holds a private string *s*, and the client wants to learn whether *s* is in the blocklist *B* without revealing its private string *s* to the servers.

The technical components of Checklist are:

* a new two-server private-information-retrieval protocol (the `pir/` directory),
* a technique that extends preprocessing offline/online PIR schemes to support database updates (see the `updatable/` directory) **==> Removed from this repository**, and 
* a Safe-Browsing service proxy that allows Firefox to perform Safe Browsing API lookups via Checklist (see the `cmd/sbproxy/` and `cmd/rpcserver/` directories). **==> Removed from this repository**

### Code organization 

The directories in this repository are:

| **Core PIR library** ||
| :--- | :---|
| [pir/](pir/) | Core PIR protocol code |
| [psetggm/](psetggm/) | Optimized C++ implementation of puncturable-set primitive|


### PIR library

Our implementation supports the following PIR protocols, which we implement in the `pir/` directory. In the bulleted list below, λ ≈ 128 is the security parameter and n is the number of rows in the database. We ignore leading constants.

* `pir.Punc` - Checklist's new two-server offline/online PIR scheme. The PIR scheme has offline server time λn, offline communication λn^{1/2}, online server time n^{1/2}, and online communication λlog(n).
* `pir.Matrix` - A simple two-server PIR scheme based on the original PIR paper of [Chor, Goldreich, Kushilevitz, and Sudan](http://www.wisdom.weizmann.ac.il/~oded/PSX/pir2.pdf). The PIR scheme has offline server time 0, offline communication 0, online server time n, and online communication n^{1/2}.
* `pir.NonPrivate` - Fetch a database record with no privacy.