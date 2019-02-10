
.. code:: ipython3

    from itertools import product
    
    def get_counts(num_dice):
        # Initialize counts
        counts = {k:0 for k in range(1, 5+1)}
    
        # Exhaustively test all combinations
        for rolls in product(range(1, 7+1), repeat=num_dice):
            # Roll 4 dice, add them up
            x = sum(rolls)
            x = x % 5
            x = x + 1
            # Update counts
            counts[x] += 1
    
        return counts
    
    
    results = {}
    for num_dice in range(1,8+1):
        results[num_dice] = get_counts(num_dice)
        
    results




.. parsed-literal::

    {1: {1: 1, 2: 2, 3: 2, 4: 1, 5: 1},
     2: {1: 9, 2: 9, 3: 10, 4: 11, 5: 10},
     3: {1: 70, 2: 68, 3: 67, 4: 68, 5: 70},
     4: {1: 481, 2: 483, 3: 481, 4: 478, 5: 478},
     5: {1: 3357, 2: 3360, 3: 3365, 4: 3365, 5: 3360},
     6: {1: 23532, 2: 23524, 3: 23524, 4: 23532, 5: 23537},
     7: {1: 164718, 2: 164718, 3: 164705, 4: 164697, 5: 164705},
     8: {1: 1152945, 2: 1152966, 3: 1152979, 4: 1152966, 5: 1152945}}



.. code:: ipython3

    import numpy as np
    import matplotlib.pyplot as plt
    
    def make_histogram(results, num_dice):
        counts = results[num_dice]
        total = sum(counts.values())
            
        fig,ax = plt.subplots(figsize=(10,7), dpi=100)
        rng = np.arange(1, 5+1)
        ax.set_xticks(rng)
        ax.set_xticklabels(rng)
        
        ax.set_title(f"Distribution, {num_dice} dice")
        ax.set_xlabel("Result")
        ax.set_ylabel("Occurrances")
        
        ax.bar(*zip(*counts.items()))
        
        (xlim_min, xlim_max) = plt.xlim()
        (ylim_min, ylim_max) = plt.ylim()
        
        def annotate_bar(val):
            lines = [
                "%8.4f %%" % (100*counts[val]/total),
                "",
                f"{counts[val]:,}",
                f"of {total:,}"
            ]
            s = '\n'.join(lines)
            y = ylim_max * 0.01
            plt.annotate(s, (val,y), ha='center', va='bottom', weight='bold', size=12)
        
        for k in counts:
            annotate_bar(k)
        
    
    make_histogram(results, 8)



.. image:: output_1_0.png

