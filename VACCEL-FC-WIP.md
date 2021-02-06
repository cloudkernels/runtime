### Firecracker/vAccel Port to Kata Rutime

This is a WiP fork that ports [Firecracker-v0.23.1/vAccel](https://github.com/cloudkernels/firecracker/tree/vaccel-v0.23.1) to kata-runtime 1.12

Until now, it has only been tested with containerd shim v2. We can run Kata Containers in Firecracker/vAccel with the following changes/issues. 

Changes related to vAccel:

- fcAddCryptoDev() adds the crypto parameters in fcConfig.json as Firecracker-v0.23.1/vAccel expects.
- seccomp level is set to 0 (Firecracker/vAccel binary is dynamically linked with vAccel libraries)
- fcDiskPoolSize value changed from 8 to 5. Firecracker has a max limit of virtio devices. (vaccel-virtio + 8 > max)
 
The kata-agent running inside the VM is also patched to expose the Guest's vaccel-virtio device to the host. Check our fork [here](add link)

We are currently working on the following issues:

- Fix the fcSetLogger function to support the newer (v0.23.1) Logger and Metrics models (disabled for now - no metrics fifo is created)
- In the current implementation, execution with containerd-shim-v2 requires Debug mode or Jailed execution (--deamonize flag). We change this to allow non debug mode, without having to use the jailer.
- explore jailed execution
- fix issues when using docker.



