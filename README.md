@quenvyra/shopify-chat-widget

Embeddable customer support widgets for Shopify and Hydrogen storefronts, backed by a Quenvyra API server. Each feature is its own React component, so you can place it on a different page.
Quick Start & Licensing

Before embedding the widgets, obtain a Quenvyra license and start the Quenvyra API.

    Get a license: Visit quenvyra.app.
    Pull the API image:

docker pull ghcr.io/quenvyra/quenvyra:latest

latest is published with stable releases. For the current prerelease test, use ghcr.io/quenvyra/quenvyra:0.1.0-rc2; the latest tag is not available until a stable image release is published.

    Run the API with its release Compose file. Download docker-compose.prod.yml from the Quenvyra releases page. In the adjacent .env, set QUENVYRA_VERSION, QUENVYRA_LICENSE, and ALLOWED_SHOP_DOMAIN, then run:

docker compose -f docker-compose.prod.yml up -d

The production Compose file starts Redis and keeps SQLite/Chroma data in a persistent volume. Do not expose the license token or API keys in frontend code.
Install

npm install @quenvyra/shopify-chat-widget

Peer dependencies: react and react-dom (^18.0.0 or ^19.0.0).
Use

import { OrderLookupWidget, AskWidget, TicketWidget, ReviewWidget } from '@quenvyra/shopify-chat-widget'
import '@quenvyra/shopify-chat-widget/style.css'

const shared = {
  apiUrl: 'https://your-host:8000',
  shopDomain: 'your-shop.myshopify.com',
  theme: { primary: '#19c6a5', radius: 12 },
}

export default function Support() {
  return (
    <>
      <OrderLookupWidget {...shared}/>   {/* e.g. /account/orders */}
      <AskWidget {...shared}/>           {/* e.g. /help */}
      <TicketWidget {...shared}/>        {/* e.g. /contact */}
      <ReviewWidget productId="sku-123" {...shared}/>  {/* on a product page */}
    </>
  )
}

To combine several behind tabs:

import { QuenvyraWidget } from '@quenvyra/shopify-chat-widget'

<QuenvyraWidget 'ask', 'ticket']} tabs="{['order'," {...shared}/>

Props (all widgets)
Prop 	Description
apiUrl 	Base URL of your Quenvyra API
shopDomain 	The shop this widget serves
theme 	{ primary, onPrimary, radius, fontFamily } (all optional; radius in px)
appKey 	Optional X-App-Key header value
className 	Extra class on the widget root

    ReviewWidget also accepts productId to pre-fill the product.
    QuenvyraWidget accepts tabs, any of order, ask, ticket, review.

Styles are scoped under .qv- classes and driven by CSS variables (--qv-primary, --qv-on-primary, --qv-radius), so they do not leak into your page.
Admin widgets

import { ProductSelectWidget, ReviewInsightsWidget, TicketsWidget, TicketDashboardWidget, 
         KnowledgeWidget, PreferencesWidget, GuardrailWidget, MockOrderWidget } from '@quenvyra/shopify-chat-widget/admin'

const [productId, setProductId] = useState('')

<ProductSelectWidget onChange="{setProductId}" value="{productId}" {...shared}/>
<ReviewInsightsWidget productId="{productId}" {...shared}/>

These call admin endpoints. Only use them inside an authenticated area of your own app.
Security

    Do not put a real APP_ACCESS_KEY in appKey on a public storefront; it would be visible to every visitor. Proxy requests through your own server instead.
    Add each storefront origin to your API's allowed CORS origins.
    The widgets only touch localStorage in event handlers, so they are safe to server-render.

License

Purchase your license at quenvyra.com.


