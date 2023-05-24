Please add PalSAT (Parallel Simple Portfolio Version) results to the benchmarks list. YalSAT (Sequential Standalone Version) is single threaded only and it skews the results in favor of Dynex.

The added benchmark continue the trend of cherry picking results. They display the YalSAT solver as being inferior, likely as a response to a [video displaying the performance of YalSAT vs Dynex](https://www.youtube.at/watch?v=kh3xYDh3uXc). This was quite embarrassing, so it seems the team has added more data to prove their point that:

> YalSat is a local-search solver using a variation of WalkSAT, a random-walk algorithm. This category of solvers is sometimes able to have a “lucky shot” at solving problem instances due to its random walk nature. Also there it becomes clear that local-search does not provide a robust and consistent way of solving benchmark instances.  --- Sumitomo on Dynex Discord

There is no doubt here regarding the results for YalSAT vs. Dynex. But the real issue is that they are comparing apples to oranges. As shown in the video the YalSAT solver is only using a single CPU thread. So it is not doing work at all in parallel. On the other hand the Dynex SAT solver is using 8 threads as [displayed in the source code](https://github.com/dynexcoin/Dynex-Neuromorphic-Chip/blob/main/dynex.cc#L103). So YalSAT is essentially outnumbered 8 to 1.

What "Sumitomo" does not seem to take into account is that there is a parallel version of YalSAT included in the package. This is called PalSAT and can be executed from the standard build with `./palsat -t THREAD_COUNT`.  Due to nature of both solutions the results are not always coherent, but with PalSAT the results continue to paint a bleak picture for Dynex and their claims of incredible performance for SAT solving. This lines up with our view that [this is all the Dynex team really has](https://bitcointalk.org/index.php?topic=5416078.msg62250680#msg62250680). So far they've shown they have an exotic SAT solver they've sourced from somewhere. Plus [the Dynex solution might be have patent issues](https://bitcointalk.org/index.php?topic=5416078.msg62227313#msg62227313).

With the Dynex data set PalSAT on 8 threads is on average over 2.6 times faster than Dynex SAT solver:

<img src="https://raw.githubusercontent.com/ares-austria/dynex-random/main/PalSAT%201.0.1%20vs%20Dynex%20(execution%20time%20in%20seconds)-min.png">

Here you can see the results in the same format as they have then on their index page:

```
PROBLEM INSTANCE                                                  Max.Walltime     PalSAT 1.0.1         Dynex
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_001.cnf    15 minutes       13.107s              48.670s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_002.cnf    15 minutes       4.179s               3.460s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_004.cnf    15 minutes       4.947s               9.692s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_005.cnf    15 minutes       3.591s               10.367s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_007.cnf    15 minutes       4.948s               10.982s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_008.cnf    15 minutes       4.948s               3.674s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_014.cnf    15 minutes       8.403s               11.576s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_016.cnf    15 minutes       2.458s               9.712s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_018.cnf    15 minutes       2.212s               12.689s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_020.cnf    15 minutes       4.186s               11.657s
transformed_barthel_n_100000_r_8.000_p0_0.080_instance_024.cnf    15 minutes       6.415s               17.092s
```

Again, if in doubt - you can run your own benchmarks with PalSAT, included in [the YalSAT repository](https://github.com/arminbiere/yalsat):

```
./palsat -t 8 ../Dynex-Neuromorphic-Chip/cnf/transformed_barthel_n_100000_r_8.000_p0_0.080_instance_001.cnf
```
