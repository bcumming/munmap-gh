compile the code:
```
$ nvcc test_munmap.cu -o munmap
```

On Santis we see the following output:
```
$ ./munmap 1000 8e9
#TLB shootdowns: 1000 with page size 65536
&&&& PERF new_no_touch_alloc 0.00306785
&&&& PERF new_no_touch_release 4.25265
&&&& PERF mmap_no_touch_alloc 0.00145759
&&&& PERF mmap_no_touch_release 4.2351
