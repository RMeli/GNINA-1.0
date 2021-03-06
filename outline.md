# Introduction:
	1. Molecular Docking
		1. Sampling & Scoring
			- Sample available space of ligand within the rigid receptor, necessary to cover conformational landscape to find energy minima
			- Scoring to evaluate sampled conformations
		2. Binding Mode
			- Essential for: binding affinity, lead optimization
		3. Binding Affinity
			- Useful for virtual screening
			- Determine if useful for experimental analysis
		4. Speed of Calculation
			- Fast = key for drug discovery
			- balancing act of speed and accuracy
	2. Scoring Functions
		1. Empirical
			- Background
				- Based on protein-ligand structure properties
				- Combination of Knowledge Based with Force Field
                - Largest proportion of all SFs
			- Pros
				- less prone to overfitting
				- can extract what contributes to score
			- Cons
				- Hard to determine where errors come from
				- Does not generalize well if tuned parameters
		2. Knowledge Based
			- Background
				- Use known crystal structures for finding occurences of interactions
				- one method: Create PMF using Boltzmann dist, essentially becomes empirical function
			- Pros
				- Generalizes well
				- Quick computation on test time
			- Cons
				- Requires large database of known structures
				- Underestimate solvent effects
				- Difficult to understand meaning
		3. Limit of all of "Normal" SF
            - Assume linear relationship betwen features and binding affinity
            - Need explicit featurization of molecules
	3. Machine Learning for Scoring
		1. Learns arbitrary function (relationships between observations)
		2. Explosion of Data allowing
	4. Related Works
        1. Traditional ML
            - Random Forest (RF-Score, SFCScore, etc.)
                - RF-Score iterated from v1 (essentially knowledge based SF) to v3 which includes features from v1 and more features from AutoDock Vina
                - SFCScore improve upon empirical SF, move away from linear modelling of relationship
                - Focus on binding aff prediction, RF-Score-VS for virtual screening
                - $\Delta_{vina}\mathrm{RF}_{20}$, parametrize corrections for Vina. (Sum of Autodock Vina score and correction term) Optimizing correction term may be way to go
            - SVM (high D variables for small datasets) (SVR-SFs, ID-Score, SVT-KB & SVR-EP)
                usually more target specific models
            - ANNs
                - NNScore ( first, classification of good/bad bindgers)
                - BgN and BsN -Score for ensemble ANNs
            - GBDT (ensemble method, XGBoost winner for several ML competitions)
                - BT-Dock only trained for docking, but better than conventional SFs
                - ESPH T-Bind
            - Deep Learning (more hidden layers, ReLU, dropout)
                - CNN most popular DL method
                - CNN VS
                    - AtomNet, DeepVS, DenseFS, Ragoza et. al
                - CNN Bind Aff
                    - PotentialNet, TopologyNet, K_{DEEP}, Fafnucy
                - CNN Binding Pose
                    - MathDL uses GAN for binding pose prediction
	5. Brief Summary of Method


# Methods:
    1. Data
        1. Redocking (PDBbind2019 refined)
        2. Crossdocking (Carlos Crossdock Dataset )
        3. Filtering
            - readable by rdkit and ProDy
            - extract ligand from protein
            - remove heteroatoms (metals, other crystalized small molecules) 
            - remove water
        4. Subsampling (Crossdocking)
        5. Benchmarking Data (PDBbind Core)
	2. Background of Gnina workings
        1. Smina is a fork of Vina, Gnina is a fork of Smina
        2. 
    3. Gnina/Smina comparison
        1. Gnina without cnn for scoring is same as smina
        2. Floating point precision (GPU)
    4. Selecting the optimal model (selection of new default ensemble)
        - Greedy selection process (Forward Algorithm)
        - Aim of selection
            - Maximal Performance
            - Fit on 4 GB of RAM
        - Iterative process
    5. Optimal Running method (Refine vs Rescore)
        1. What does refinement do?
        2. What does rescoring do?
        3. CNN empirical weight
    6. Exploration of Settings (Selecting Default Settings)
        1. Defined Pocket
            - Exhaustiveness 
            - CNN Rotations 
            - Min RMSD Filter 
            - Num MC Saved 
            - Number of Modes
         2. Whole Protein
             - Exhaustiveness
# Results
    1. Gnina and Smina are the same
        - Figures in Supplement (bc they ugly)
        - Slight performance boost with floating point precision
    2. Default Model selection
        - Compare performance to single models/Vina
        - Compare performance to ensemble models/Vina
        - Pareto Optimal Curve
    3. Refinement vs. Rescoring
        - Compare results Fig
        - Pareto Optimal Curve
    4. Settings Exploration
        1. Defined Pocket
            - Important parameters:
                - Exhaustiveness
                - Autobox Add
                - Number of Modes
                - Number Monte Carlo Saved
            - Optimal settings
        2. Whole Protein
            - Important parameters:
                - Exhaustiveness
            - Optimal Settings:
    5. Performance of Gnina:
        - Timesplit results:
            - PDBbind2019 refined with no PDBbind2017 general
                - Training set for what?
            - PDBbind2019 refined with no PDBbind2017 general AND no CD2020
                - Training set for CD and Dense
        - CNNscore
            - Threshold provide better results
