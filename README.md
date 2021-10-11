 
# CiiCube to Rust

This a direct translation from the [project](https://github.com/Blaldas/CiiCube) created by [Blaldas](https://github.com/blaldas).


The objective for this project is to see how does Rust compare to Java in this specific application, being it speed and memory usage.


In mean results are:

| Language | Time to load | Memory Usage (only program) | Memory usage (with vm) |
| -------- | ------------ | --------------------------- | ---------------------- |
| Rust     | ~954,46ms    | 39.16MB                     | -                      |
| Java     | 1042,6ms     | ~30MB                       | ~120MB                 |


## Observations

Rust had a constant use of memory while Java fluctuated in memory usage and with free space not considered in the only program and in the *with vm*  considered all the Runtime memory usage.

In conclusion Java can be nearly as fast as rust but still has the overhead of running in a Interpreter that takes more memory and cpu.


## Metodology

for registiring the times for the java:
```bash
for i in {i..35} ; do clearbuffer; echo exit | java notUsingFastUtil.Main ../../../project4rust/datasets/datas | grep Miliseconds  | tr -s ' ' | tr '\t' ' '|cut -d ' ' -f 7 >> ../../../project4rust/benchmarks/time_java ; done
```

for registering the memory in java I used the original code from @Blaldas and used this command:

```bash
for i in {1..35}; do clearbuffer; echo exit | java notUsingFastUtil.Main ../../../project4rust/datasets/datas | grep used | tr -s ' ' | tr '\t'  ' ' | cut -f 4 -d ' ' >> ../../../project4rust/benchmarks/memory_no_vm ; done 
```

after that I changed the program to report all the memory used in the runtime saving it in a similar way:

```bash
for i in {1..35}; do clearbuffer; echo exit | java notUsingFastUtil.Main ../../../project4rust/datasets/datas | grep used | tr -s ' ' | tr '\t'  ' ' | cut -f 4 -d ' ' >> ../../../project4rust/benchmarks/memory_with_vm ; done 
```

for rust, the speed recording is the same as java:
```bash
for i in {1..35} ; do clearbuffer ;  echo exit | ./target/release/project4rust datasets/datas | grep Milliseconds | tr -s ' ' | tr '\t'  ' ' | cut -f 7 -d ' ' >> benchmarks/time_rust ; done
```

and finally for memory usage in rust:

```bash
for i in {1..35} ; do clearbuffer; echo exit | ./target/release/project4rust datasets/datas | grep memory -i | tr -s ' ' | tr '\t'  ' ' | cut -f 4 -d ' ' | replace 'K' "000" >> benchmarks/memory_rust ; done
```

For getting the means of the files i used the program `datamash` from the gnu utilities:
```bash
cat benchmarks/benchmark | datamash mean 1
```
it was used for every benchmark


### Notes

`clearbuffer` is a script that cleans up the cache created by the os:
```bash
sync; echo 3 > /proc/sys/vm/drop_caches
```

All the benchmarks results are in the folder `benchmarks` and the datasets are in the `datasets` folder.