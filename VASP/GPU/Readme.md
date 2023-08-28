# Running a basic VASP calculation on GPUs
using longleaf at UNC  
/proj/averittlab/apps/  * where VASP is installed 
/proj/averittlab/modulefiles  *where modules are installed

For the purposes of running on a GPU you need to use SBATCH flags to get a GPU for your job. I have a sample job submission script below. VASP mentions a few pointers here:   
[Click here and look under "Running the OpenACC version"](https://www.vasp.at/wiki/index.php/OpenACC_GPU_port_of_VASP )
  
[also try here : Combining_MPI_and_OpenMP](https://www.vasp.at/wiki/index.php/Combining_MPI_and_OpenMP )

My suggestion is that you do some test runs using a GPU and while you are doing these tests you monitor the GPU usage of your job to see how and if it's using the GPU:
 
[monitor the GPU usage of your job](https://help.rc.unc.edu/gpumonitor/)   
or whatever your cluster uses

PS. Note that I arbitrarily set the number of CPU threads in the job to 8. You may need to tinker around with different values. It's possible that your job may not even benefit from CPU threading, but the VASP documentation referenced above indicates that VASP will likely benefit from CPU threading. The value used for “--cpus-per-task” and the OMP_NUM_THREADS environment variable should differ by one as below (I think).

## batch.sh file 

<pre>
#!/bin/bash

#SBATCH -N 1
#SBATCH -n 1
#SBATCH --cpus-per-task=9
#SBATCH -p beta-gpu
#SBATCH --qos gpu_access
#SBATCH --gres=gpu:1
#SBATCH -t 5-
 
module add vasp/6.3.2
 
export OMP_NUM_THREADS=8
export MKL_THREADING_LAYER=INTEL
 
mpirun -np 1 <your-vasp-executable>
</pre>




