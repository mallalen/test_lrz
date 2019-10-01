#!/bin/bash
# Job Name and Files (also --job-name)
#SBATCH -J test
#Output and error (also --output, --error):
#SBATCH -o test_.out
#SBATCH -e test_.out
#Initial working directory (also --chdir):
#SBATCH -D ./
#Notification and type
#SBATCH --mail-type=NONE
#SBATCH --mail-user=xxxx@XXX
# Wall clock limit:
#SBATCH --time=00:30:00
#SBATCH --ntasks=192   # number of processor cores (i.e. tasks)
#SBATCH --nodes=4
#Setup of execution environment
##SBATCH --export=NONE
#constraints are optional

source /etc/profile.d/modules.sh

for i in `scontrol show hostname $SLURM_NODELIST`; do
  for j in $(seq 1 $SLURM_TASKS_PER_NODE); do echo $i >> farming.machines; done
done
L1=1
L2=170
sed -n -e "${L1},${L2}p" ./farming.machines >mfile.1
mpiexec -n 4 -f mfile.1 sleep 2

L1=171
L2=180
sed -n -e "${L1},${L2}p" ./farming.machines >mfile.2
mpiexec -n 4 -f mfile.2 sleep 2

L1=181
L2=192
sed -n -e "${L1},${L2}p" ./farming.machines >mfile.3
mpiexec -n 4 -f mfile.3 sleep 2

echo "Executed in diff seconds"
