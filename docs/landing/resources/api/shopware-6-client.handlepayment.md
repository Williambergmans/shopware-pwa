<!-- Do not edit this file. It is automatically generated by API Documenter. -->

[Home](./index.md) &gt; [@shopware-pwa/shopware-6-client](./shopware-6-client.md) &gt; [handlePayment](./shopware-6-client.handlepayment.md)

## handlePayment() function

<b>Signature:</b>

```typescript
export declare function handlePayment(orderId: string, finishUrl?: string, errorUrl?: string, contextInstance?: ShopwareApiInstance): Promise<{
    redirectUrl: string | null;
    apiAlias: string;
}>;
```

## Parameters

|  Parameter | Type | Description |
|  --- | --- | --- |
|  orderId | string | Id of an order |
|  finishUrl | string | URL where the customer is redirected to after payment is done |
|  errorUrl | string | URL where the customer is redirected to after payment fails |
|  contextInstance | [ShopwareApiInstance](./shopware-6-client.shopwareapiinstance.md) |  |

<b>Returns:</b>

Promise&lt;{ redirectUrl: string \| null; apiAlias: string; }&gt;

