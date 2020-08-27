# Visualizing Rules

## Heatmaps
1. Generate the rules using Apriori and association rules
2. Convert antecendents and consequents into strings
3. Convert rules into matrix format that is suitable for heatmaps

```python
# Apply the apriori algorithm
frequent_itemsets = apriori(onehot, min_support=0.10, use_colnames=True, max_len=2)
```
```python
# Recover the association rules
rules = association_rules(frequent_itemsets)
```
```python
# Convert antecendents and consequents into strings
rules['antecedents'] = rules['antecedents'].apply(lambda a: ','.join(list(a)))
rules['consequents'] = rules['consequents'].apply(lambda a: ','.join(list(a)))
```
```python
# Transform antecendents, consequents, and support columns into matrix
support_table = rules.pivot(index='consequents', columns='antecedents', values='support')
```

```python
# Generate heatmap
sns.heatmap(support_table, annot=True, cbar=False, cmap='ocean')
```