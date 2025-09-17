-> 
import matplotlib.pyplot as plt
import numpy as np
plt.rcParams.update({'font.size':15})

# Create figure

fig = plt.figure(figsize=(20, 15))

gs = plt.GridSpec(4, 3, width_ratios=[2, 2, 2], height_ratios=[2, 2, 4, 4])

  
  

# 3. histogram of performance

ax3 = fig.add_subplot(gs[:2, 2:3])

ax3.hist(res_df["overall Performance"], bins=np.arange(0, 1.01, 0.01),

color='black')

ax3.set_title("Grid Search Performance Distribution")

  

# 4. train iterations vs performance

ax1 = fig.add_subplot(gs[0, :2])

train_it_mean_res = res_df.groupby("train_iterations").mean(numeric_only=True)

train_it_std_res = res_df.groupby("train_iterations").std(numeric_only=True)

  

ax1.errorbar(

x=train_it_mean_res.index,

y=train_it_mean_res["overall Performance"],

yerr=train_it_std_res["overall Performance"],

c='black'

)

ax1.set_title("Train Iterations vs Performance")

ax1.set_ylim(0, 1)

  

# 5. test iterations vs performance

ax2 = fig.add_subplot(gs[1, :2])

test_it_mean_res = res_df.groupby("test_iterations").mean(numeric_only=True)

test_it_std_res = res_df.groupby("test_iterations").std(numeric_only=True)

  

ax2.errorbar(

x=test_it_mean_res.index,

y=test_it_mean_res["overall Performance"],

yerr=test_it_std_res["overall Performance"],

c='black'

)

ax2.set_title("Test Iterations vs Performance")

ax2.set_ylim(0, 1)

  
  

# 1. n_total vs performance

ax8 = fig.add_subplot(gs[3, 1])

ntot_res_mean = res_df.groupby("train_n_total").mean(numeric_only=True)

ntot_res_std = res_df.groupby("train_n_total").std(numeric_only=True)

  

ax8.scatter(

x=ntot_res_mean.index,

y=ntot_res_mean["overall Performance"],

c='black')

  

ax8.errorbar(

x=ntot_res_mean.index,

y=ntot_res_mean["overall Performance"],

yerr=ntot_res_std["overall Performance"],

c='black'

)

ax8.set_title("n cells in mixture (train) vs Performance")

ax8.set_ylim(0, 1)

  

# 2. runs_training vs performance

ax4 = fig.add_subplot(gs[2, 0])

rt_res_mean = res_df.groupby("runs_training").mean(numeric_only=True)

rt_res_std = res_df.groupby("runs_training").std(numeric_only=True)

ax4.scatter( x=rt_res_mean.index, y=rt_res_mean["overall Performance"],

c='black')

ax4.errorbar(

x=rt_res_mean.index,

y=rt_res_mean["overall Performance"],

yerr=rt_res_std["overall Performance"],

c='black'

)

ax4.set_title("Training runs vs Performance")

ax4.set_ylim(0, 1)

  
  
  
  

# 6. test learning rate vs performance

ax5 = fig.add_subplot(gs[2, 1])

test_lr_mean_res = res_df.groupby("test_lr").mean(numeric_only=True)

test_lr_std_res = res_df.groupby("test_lr").std(numeric_only=True)

  

ax5.scatter(

x=test_lr_mean_res.index,

y=test_lr_mean_res["overall Performance"],

c='black')

ax5.errorbar(

x=test_lr_mean_res.index,

y=test_lr_mean_res["overall Performance"],

yerr=test_lr_std_res["overall Performance"],

c='black'

)

ax5.set_xscale("log")

ax5.set_title("Test Learning Rate vs Performance")

ax5.set_ylim(0, 1)

  

# 7. train learning rate vs performance

ax6 = fig.add_subplot(gs[2, 2])

train_lr_mean_res = res_df.groupby("train_lr").mean(numeric_only=True)

train_lr_std_res = res_df.groupby("train_lr").std(numeric_only=True)

  

ax6.scatter(

x=train_lr_mean_res.index,

y=train_lr_mean_res["overall Performance"],

c='black')

ax6.errorbar(

x=train_lr_mean_res.index,

y=train_lr_mean_res["overall Performance"],

yerr=train_lr_std_res["overall Performance"],

c='black'

)

ax6.set_xscale("log")

ax6.set_title("Train Learning Rate vs Performance")

ax6.set_ylim(0, 1)

  

# 8. PatPerBatch/n_batches vs performance (from res_df2)

ax7 = fig.add_subplot(gs[3, 0])

ppb_res_mean = res_df2.groupby("ppb_n_batches").mean(numeric_only=True)

ppb_res_std = res_df2.groupby("ppb_n_batches").std(numeric_only=True)

ppb_res_mean = ppb_res_mean.sort_values(by="train_n_batches")

ppb_res_std = ppb_res_std.sort_values(by="train_n_batches")

ax7.scatter(x=ppb_res_mean.index,

y=ppb_res_mean["overall Performance"],

c='black')

ax7.errorbar(

x=ppb_res_mean.index,

y=ppb_res_mean["overall Performance"],

yerr=ppb_res_std["overall Performance"],

c='black'

)

ax7.set_title("PatPerBatch / n_batches vs Performance")

ax7.set_ylim(0, 1)

  

ax9 = fig.add_subplot(gs[3,2])

  

ax9.scatter(x=lam_res_mean.index, y=lam_res_mean["overall Performance"],

c='black')

ax9.errorbar(x=lam_res_mean.index, y=lam_res_mean["overall Performance"], yerr=lam_res_std["overall Performance"],

c='black')

ax9.set_title("Train lambda vs Performance")

ax9.set_ylim(0, 1)

ax9.set_xscale('log')

  
  

import string

  

# Collect all axes you created

axes = [ax1, ax2, ax3, ax4, ax5, ax6, ax7, ax8, ax9]

for i, ax in enumerate(axes):

h = 1.10

if ax == ax1 or ax == ax2:

h = 1.20

if ax == ax3:

h = 1.07

ax.text(

0, h, # position relative to axis (x,y)

f"({string.ascii_uppercase[i]})", # (a), (b), (c), ...

transform=ax.transAxes, # relative to axis

fontsize=20,

fontweight="bold",

va="top",

ha="right"

)

plt.tight_layout()

plt.show()

  

fig.savefig("/mnt/results/results_DynaMiCs/results_gridsearch/figures2/combined_grdsearch_results.png", dpi=400)