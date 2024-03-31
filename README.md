const app = express();
const PORT = process.env.PORT || 3000;
const pricings = [
 { organization_id: '005', zone: 'central', base_distance_in_km: 5, km_price: 0.015, 
fix_price: 1000 }
];
app.use(bodyParser.json());
function calculatePrice(zone, organization_id, total_distance) {
 const pricing = pricings.find(p => p.organization_id === organization_id && p.zone === zone);
 if (!pricing) {
 return null; 
 }
 const base_distance = pricing.base_distance_in_km;
 const km_price = pricing.km_price;
 const fix_price = pricing.fix_price;
 const total_price = fix_price + (total_distance - base_distance) * km_price;
 const total_price_in_cents = Math.round(total_price * 100); 
 return { total_price: total_price_in_cents };
}
app.post('/calculatePrice', (req, res) => {
 const { zone, organization_id, total_distance } = req.body;
 const response = calculatePrice(zone, organization_id, total_distance);
 if (response === null) {
 return res.status(404).json({ error: 'Pricing not found for provided organization and zone' });
 }
 res.json(response);
});
app.listen(PORT, () => {
 console.log(Server is running on port${PORT});

<!---
Gokul9344/Gokul9344 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
