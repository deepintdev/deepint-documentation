# Using iframe tokens for dashboards or visualizations

In order to display visualizations or dashboards in external websites, you can use the Iframe tokens.

First, you need to set up the workspace secret. This secret is used to sign the tokens, so if you change it, any token you created with it will become invalid. 

In order to change the workspace secret, use the workspace modification API:

 - [https://app.deepint.net/api/v1/documentation/#/workspaces/post_api_v1_workspace__workspaceId_](https://app.deepint.net/api/v1/documentation/#/workspaces/post_api_v1_workspace__workspaceId_)

In order to create the token, use the iframe API:

 - [https://app.deepint.net/api/v1/documentation/#/workspaces/post_api_v1_workspace__workspaceId__iframe](https://app.deepint.net/api/v1/documentation/#/workspaces/post_api_v1_workspace__workspaceId__iframe)

When creating the token, you can set a list of filters to apply, in order to restrict the data.

The URL you will get will be like this:

```
https://app.deepint.net/iframe/workspace-id?token=TOKEN
```

You can add extra params to the URL to customize the style:

| Parameter | Description |
|---|---|
| `theme` | Set to `dark` for dark theme. Set to `light` for light theme. |
| `lang` | Forced language. By default it uses the browser language. |
| `style` | Dashboard style. Can be `default` to show headers and controls, `controls` to hide header or `clean` to only display sections |




