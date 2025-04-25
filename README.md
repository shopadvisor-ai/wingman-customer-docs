# Wingman Customer Docs

### Shopify Installation code

**Note** : Replace the store id before sending it to the customet
```javascript
<script type="module" src="https://widget.getwingman.io/?store_id=your_store_id">
</script> 
```

### Shopify CORS Support

```javascript
 <meta http-equiv="Content-Security-Policy" content="script-src 'self' *; connect-src *; media-src * blob:; img-src *;">

	<link rel="canonical" href="{{ canonical_url }}">
    <link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
	<link rel="dns-prefetch" href="https://{{shop.domain}}" crossorigin>
	<link rel="dns-prefetch" href="https://{{shop.permanent_domain}}" crossorigin>
	{%- unless settings.header_font.system? and settings.body_font.system? and settings.fonts == '2' -%}
      <link rel="preconnect" href="https://fonts.shopifycdn.com" crossorigin>
```

### Customer event

```javascript
// Step 1. Initialize the JavaScript pixel SDK (make sure to exclude HTML)


// Step 2. Subscribe to customer events with analytics.subscribe(), and add tracking
//  analytics.subscribe("all_standard_events", event => {
//    console.log("Event data ", event?.data);
//  });
analytics.subscribe("all_events", event => { 
  console.log("Event data", event)
});



async function sendAttributionData(eventName, event) {
  const attribute_id = await browser.cookie.get('wingman_attribution_id')
  console.log("Id", attribute_id)

  await fetch('https://apiv2-prod.getwingman.io/api/v1/sales/create', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      attribution_id: attribute_id,
      eventName: eventName,
      eventData: event
    }),
    keepalive: true
  });
}

analytics.subscribe("checkout_started", async (event) => { 
  console.log("Got checkout started 1")
  await sendAttributionData("checkout_started", event);
});

analytics.subscribe("checkout_completed", async (event) => {
  console.log("Got checkout completed 1")
  await sendAttributionData("checkout_completed", event);
});

```
