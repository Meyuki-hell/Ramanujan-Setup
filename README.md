# Ramanujan-Setup


Here are the exact commands in order every time you reconnect:

Step 1 — Your laptop terminal
bashssh minal.t@172.16.176.120

Step 2 — On jiit-master, request GPU node
bashmodule purge
module add cuda12.2/toolkit/12.2.2
module add cudnn/9.14
module add anaconda3/anaconda
conda activate eefgan
qsub -I -q gpu -l select=1:ncpus=8:mem=64gb -l walltime=12:00:00
Wait for prompt to change to [minal.t@jiit-gpu01 ~]$

Step 3 — On jiit-gpu01, load environment
bashmodule purge
module add cuda12.2/toolkit/12.2.2
module add cudnn/9.14
module add anaconda3/anaconda
conda activate eefgan

Step 4 — Start Jupyter
bashcd /home/minal.t/EEF_GAN
jupyter notebook --no-browser --port=8899 --ip=0.0.0.0
Copy the token from output.

Step 5 — New terminal on laptop (SSH tunnel)
bashssh -L 8899:jiit-gpu01:8899 minal.t@172.16.176.120
Open browser → http://localhost:8899 → paste token ✅

Save this as a script so you never type it again
On jiit-master, run this once:
bashcat > ~/start_eefgan.sh << 'EOF'
#!/bin/bash
module purge
module add cuda12.2/toolkit/12.2.2
module add cudnn/9.14
module add anaconda3/anaconda
conda activate eefgan
cd /home/minal.t/EEF_GAN
jupyter notebook --no-browser --port=8899 --ip=0.0.0.0
EOF
chmod +x ~/start_eefgan.sh
Next time, after qsub lands you on jiit-gpu01, just type:
bashbash ~/start_eefgan.sh
One command and Jupyter starts. 🚀
