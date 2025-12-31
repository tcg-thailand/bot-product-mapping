# Battle of Talingchan Product Mapping

Human-readable product mapping for the Battle of Talingchan market price API.

## Files

| File | Description |
|------|-------------|
| `mapping.json` | ProductId → Card details (Name, CardNumber, Rarity, SetCode, SetName) |
| `metadata.json` | Version info and product/set counts |
| `delta.json` | Changes since previous release |

## Usage

Join with the price API response using `ProductId`:

```javascript
// API returns: { ProductId, Price, Sellers, LastSold }
// Mapping provides: { Name, CardNumber, Rarity, SetCode, SetName }

const prices = await fetch('API_URL').then(r => r.json());
const mapping = await fetch('https://raw.githubusercontent.com/YOUR_USERNAME/REPO_NAME/main/output/mapping.json').then(r => r.json());

const enriched = prices.map(p => ({
  ...p,
  ...mapping[p.ProductId]
}));
```

## Updates

- Released monthly via [GitHub Releases](../../releases)
- Use `delta.json` for incremental updates
- Subscribe to releases for notifications

## Versioning

- **Minor** (1.x.0): New set released
- **Patch** (1.0.x): Cards added/removed/modified within existing sets

## License

MIT
