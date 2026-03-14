# Agent: DORA

**Description:**  
DORA is a lightweight automation agent for dropshipping, e-commerce, and OpenClaw workflows.

**Type:** automation  
**Enabled:** true

## Capabilities
- web_browsing
- shopify_integration
- woocommerce_integration
- telegram_notifications
- data_scraping
- product_listing
- order_management
- inventory_sync
- marketing_automation
- task_scheduling

## Default Personality
PERSONALITY_DORA.md

## Settings
- maxConcurrentTasks: 2
- enableBrowser: true
- browserProfile: dora
- headlessBrowser: true
- logLevel: info

## Integration
### Shopify
- enabled: true
- apiKey: `${SHOPIFY_API_KEY}`
- password: `${SHOPIFY_PASSWORD}`
- storeUrl: `${SHOPIFY_STORE_URL}`

### WooCommerce
- enabled: true
- consumerKey: `${WC_CONSUMER_KEY}`
- consumerSecret: `${WC_CONSUMER_SECRET}`
- storeUrl: `${WC_STORE_URL}`

### Telegram
- enabled: true
- botToken: `${TELEGRAM_BOT_TOKEN_DUTYCLAW}`

## Browser Settings
- args:
  - --disable-dev-shm-usage
  - --disable-gpu
  - --single-process
  - --disable-background-networking
  - --disable-extensions
  - --disable-software-rasterizer
- maxConcurrentBrowsers: 1
