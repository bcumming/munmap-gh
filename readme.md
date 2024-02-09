compile the code:
```
$ nvcc test_munmap.cu -o munmap
```

On Santis (Alps Blanca Peak) we see the following output:

```
$ ./munmap 1000 8e9
#TLB shootdowns: 1000 with page size 65536
&&&& PERF new_no_touch_alloc 0.00306785
&&&& PERF new_no_touch_release 4.25265
&&&& PERF mmap_no_touch_alloc 0.00145759
&&&& PERF mmap_no_touch_release 4.2351
```

On Tasna (Alps Grizzly Peak)
```
$ ./munmap 1000 8e9
#TLB shootdowns: 1000 with page size 4096
&&&& PERF new_no_touch_alloc 0.00355476
&&&& PERF new_no_touch_release 0.00307908
&&&& PERF mmap_no_touch_alloc 0.00261974
&&&& PERF mmap_no_touch_release 0.0016042
```

## Commentry From NVIDIA

If you run this on an x86 system you'll see that release numbers are way smaller. That's because x86 does range page invalidation, and arm does not.
The patch that enables range invalidation for arm:

https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?h=v5.15.134&id=f5a604757aa8e37ea9c7011dc9da54fa1b30f29b

So, my suggestion to HPE would be to incorporate the kernel patch, and, if possible, to lower the threshold (`CMDQ_MAX_TLBI_OPS`) by 8x. It isn't clear that we’ll be able to upstream the lower threshold, because the kernel maintainer doesn’t want to change it arbitrarily, but if HPE uses a custom/patched kernel they can just lower this threshold.
