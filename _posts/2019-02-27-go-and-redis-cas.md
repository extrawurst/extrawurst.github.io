---
title:  "Reliable CCU with redis and golang"
date:   2019-02-27 18:00:00
categories: [general]
---

DESC

## Why it is hard

* scalability

## CAS

* compare and swap

## How CAS saves the day

1. everytime an endpoint is called by a logged in client we add his id to a hash set
2. we can poll a key `last_ccu` everytime we add a hash to our online set
3. if the timestamp in `last_ccu` is older than X (lets say 15min) we can aggregate the data
4. aggregation means that we save the number of entries in our hash set (CCU of the last 15min)
5. we update the `last_ccu` entry to the current timestamp and clear the hash set

## Scripts in Redis

```go
dat, err := ioutil.ReadFile("cas.lua")
check(err)
hash, err := redis.ScriptLoad(string(dat)).Result()
check(err)
```

```go
func cas(key string, oldVal string, newVal string) bool {

	res, err := redis.EvalSha(hashash, []string{key}, oldVal, newVal).Int()
	check(err)

	return res != 0
}
```

## Conclusion

* realworld implementation is more complex because u want to have the whole operation take place in one script (wiping the hash set aswell)
* not golang specific
* not redis specific
* eg. DynamoDB has CAS built in