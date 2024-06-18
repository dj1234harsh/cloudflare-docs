---
pcx_content_type: how-to
title: Setup Pages for a SaaS platform
---

# Setup Pages for a SaaS platform

## Background

Learn how to have customers point their domains to your Pages projects for SaaS platforms. This allows customers to use Cloudflare proxying/DNS or an external DNS provider.

## Set up

{{<Aside type="note" header="SSL for SaaS">}}
  You should not use SSL for SaaS for this situation. Trying to use SSL for SaaS will hit problems due to Pages itself using SSL for SaaS underneath.
{{</Aside>}}

{{<Aside type="note" header="Domain limit">}}
  If you need the [custom domain limit](/pages/platform/limits/#custom-domains) raised and you're an Enterprise customer please contact your account team.
  The custom domain limit is _per project_ not per account.
{{</Aside>}}

1. Pick your CNAME target, the record that you want customers to point to. In this guide, our example target will be `custoemrs.example.com`.
2. Add your CNAME target to [Pages custom domains](/pages/configuration/custom-domains/)
  ![Add your CNAME target to Pages custom domains](/images/pages/how-to/setup-pages-saas/saas-domain.png)

1. With your CNAME target setup, you can now have customers add a CNAME on their own domains to `customers.example.com`. Example:
  ![Example customer record](/images/pages/how-to/setup-pages-saas/customer-cname.png)

1. If you're handling multiple customers within the same Pages project, you will probably want to serve different content per customer. To do this, within a [Pages Function](/pages/functions/) you can check the hostname of the request. Example:
```js
export function onRequestGet(context) {
 // Get the customer domain
 const hostname = new URL(context.request.url).hostname;

 // Fetch customer details from KV
 const details = context.env.KV.get(hostname);
 // ... do things with the details (e.g. serving custom content, rewriting HTML, etc.)
}
```

## Related resources

- [Custom domain limits](/pages/platform/limits/#custom-domains)
- [SSL for SaaS](/cloudflare-for-platforms/cloudflare-for-saas)
