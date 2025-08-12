---
title: "5.3 Compare Performance"
date: 2024-01-01
weight: 3
---

# 5.3 Compare Performance

## Performance Comparison Script

```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

def compare_performance(standard_results, optimized_results):
    # Create comparison DataFrame
    comparison = pd.DataFrame({
        'Standard Cold Start': [r['duration_ms'] for r in standard_results['cold']],
        'Optimized Cold Start': [r['duration_ms'] for r in optimized_results['cold']],
        'Standard Warm Start': [r['duration_ms'] for r in standard_results['warm']],
        'Optimized Warm Start': [r['duration_ms'] for r in optimized_results['warm']]
    })
    
    # Calculate statistics
    stats = comparison.describe()
    print(stats)
    
    # Create visualization
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))
    
    # Cold start comparison
    ax1.boxplot([comparison['Standard Cold Start'], 
                 comparison['Optimized Cold Start']], 
                labels=['Standard', 'Optimized'])
    ax1.set_title('Cold Start Performance')
    ax1.set_ylabel('Duration (ms)')
    
    # Warm start comparison
    ax2.boxplot([comparison['Standard Warm Start'], 
                 comparison['Optimized Warm Start']], 
                labels=['Standard', 'Optimized'])
    ax2.set_title('Warm Start Performance')
    ax2.set_ylabel('Duration (ms)')
    
    plt.tight_layout()
    plt.savefig('performance_comparison.png')
    plt.show()
    
    return stats
```

## Performance Improvements

| Metric | Standard | Optimized | Improvement |
|--------|----------|-----------|-------------|
| Cold Start | ~3000ms | ~1500ms | 50% |
| Warm Start | ~200ms | ~100ms | 50% |
| Image Size | 500MB | 200MB | 60% |

## Key Insights

- Optimized version shows significant improvements
- Cold start reduction is most impactful
- Smaller image size improves download time