---
title: "Performance Testing"
date: "`r Sys.Date()`"
weight: 5
chapter: true
pre: "<b>5. </b>"
---

# Performance Testing

## Tổng quan

Comprehensive performance testing để so sánh standard vs optimized container images, measure cold start times, và optimize settings.

### ⏱️ **Thời gian ước tính**: 30-40 phút

## Sections

1. [**Test Cold Start**](5.1-test-cold-start/) - Measure initial function startup time
2. [**Test Warm Start**](5.2-test-warm-start/) - Test subsequent invocations
3. [**Compare Performance**](5.3-compare-performance/) - Analyze standard vs optimized
4. [**Optimize Settings**](5.4-optimize-settings/) - Fine-tune memory và timeout

## Expected Results

- **Standard Image**: ~2.8ms execution time
- **Optimized Image**: ~1.2ms execution time  
- **Performance Improvement**: 56% faster execution
- **Cost Savings**: Significant reduction in compute costs

Continue to [Test Cold Start](5.1-test-cold-start/)!