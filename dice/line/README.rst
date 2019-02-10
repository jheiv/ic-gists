
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
    
    
    def evaluate(counts):
        # Get each count's error
        total = sum(counts.values())
        emean = total / 5   # Expected mean
        deltas = {k:abs(v - emean) for (k,v) in counts.items()}
        # define errors to be deltas relative to the expected mean
        errors = {k:(v / emean) for (k,v) in deltas.items()}
        
        max_err  = max(errors.values())
        mean_err = sum(errors.values()) / len(errors)
        
        return (max_err, mean_err)
    
    
    results = []
    for num_dice in range(1,8+1):
        counts = get_counts(num_dice)
        (max_err, mean_err) = evaluate(counts)
        results.append((num_dice, max_err, mean_err))
        
    results




.. parsed-literal::

    [(1, 0.42857142857142866, 0.34285714285714286),
     (2, 0.12244897959183665, 0.06530612244897957),
     (3, 0.023323615160349774, 0.016326530612244882),
     (4, 0.005830903790087488, 0.003665139525197839),
     (5, 0.0013089784018563964, 0.0008567858630332654),
     (6, 0.00030599495108333803, 0.000197196746253693),
     (7, 7.042740937635205e-05, 4.565638952672273e-05),
     (8, 1.6305853402438842e-05, 1.0546764753899844e-05)]



.. code:: ipython3

    import matplotlib.pyplot as plt
    
    (x, y1, y2) = zip(*results)
    fig,ax = plt.subplots(figsize=(10,7), dpi=100)
    ax.semilogy(x, y1, x, y2)
    ax.set_xlabel('Number of Dice')
    ax.set_ylabel('Error')
    ax.legend(['Max Error', 'Mean Error'])
    plt.show()



.. image:: output_1_0.png

