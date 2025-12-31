# Battle of Talingchan Product Mapping

Product mapping for the Battle of Talingchan price API by TCG Thailand.

## Files

| File | Description |
|------|-------------|
| `mapping.json` | ProductId → Card details (Name, CardNumber, Rarity, SetCode, SetName) |
| `metadata.json` | Version info and product/set counts |
| `delta.json` | Changes since previous release (for incremental updates without re-downloading full mapping) |

## API

**Endpoint:** `https://api.tcgthailand.com/api/v1/public/battle-of-talingchan-price`

**Please limit requests to once per hour.**

**Authentication:** Requires `X-API-KEY` header. Contact admin@tcgthailand.com to request access.

**Response:**
```json
[{ "ProductId": "uuid", "Price": "10000", "Sellers": 5, "LastSold": "9500" }, ...]
```

| Field | Type | Description |
|-------|------|-------------|
| `ProductId` | string | Unique product identifier (UUID) |
| `Price` | string | Current lowest listed price on the market (minor unit: `10000` = 100.00 THB) |
| `Sellers` | integer | Number of sellers currently listing this product |
| `LastSold` | string | Last sold price of the product (minor unit: `10000` = 100.00 THB) |

> **Note:** `Price` and `LastSold` are strings in minor unit format. Parse to number and divide by 100 to get THB (e.g., `Number(Price) / 100`).

> **Product Page:** Use the `ProductId` to construct a direct link to the product page: `https://www.tcgthailand.com/product/{ProductId}`

## Usage

Join the API response with this mapping using `ProductId`:

```typescript
// API returns: { ProductId, Price, Sellers, LastSold }
// Mapping provides: { Name, CardNumber, Rarity, SetCode, SetName }

const prices = await fetch('API_URL', { headers: { 'X-API-KEY': 'your-key' } }).then(r => r.json());
const mapping = await fetch('https://raw.githubusercontent.com/tcg-thailand/bot-product-mapping/main/output/mapping.json').then(r => r.json());

const enriched = prices.map(p => ({
  ...p,
  ...mapping[p.ProductId]
}));
```

## Updates

- Released each time a new card is added to TCG Thailand database via [GitHub Releases](../../releases)
- Subscribe to releases for notifications
- Use `delta.json` for incremental updates

## Versioning

- **Minor** (1.x.0): New set released
- **Patch** (1.0.x): Cards added/removed/modified within existing sets
