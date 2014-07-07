##Encryption
1st Things first - NEVER EVER Store Passwords in Plain text!!! There is no excuse and too many companys are in the media these day because of stupidity and laziness...a Lazy encription is better then none.

Having said that while not storing in plain text will prevent some (the lazy or uneducated hackers) from getting information, hashing will not neccessarly be enough. Some Developers use hash functions like MD5, SHA1, SHA256, SHA512 or SHA-3, some may even combine them some how and they suddenly feel safe.....YOU DATA IS NOT SAFE!

A modern server can calculate the MD5 hash of about [330MB every second](http://www.cryptopp.com/benchmarks-amd64.html). If passwords are lowercase, alphanumeric, and 6 characters long, you can try every single possible password of that size in around 40 seconds.

Enter bcrypt...
bcrypt is a key derivation function for passwords designed by Niels Provos and David Mazières, based on the Blowfish cipher. It uses a variant of the Blowfish’s keying schedule, and introduces a work factor to determine how expensive the hash function will be. This makes it more future proof then the previously mentioned hash function because computers get faster you can increase the work factor and the hash will get slower. so instead of cracking a password in 40 seconds,it could take up to 12 years

SO lets start by installinf bcrypt
`npm install bcrypt --save`



Sources
- http://openwall.com/crypt/
- http://codahale.com/how-to-safely-store-a-password/
