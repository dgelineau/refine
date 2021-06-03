---
id: useRadioGroup
title: useRadioGroup
---

import basicUsage from '@site/static/img/guides-and-concepts/hooks/useRadioGroup/basic-usage.png';

`useRadioGroup` hook allows you to manage an Ant Design [Radio.Group](https://ant.design/components/radio/#components-radio-demo-radiogroup-with-name) component when records in a resource needs to be used as radio options.

## Example

Let's examine what `useRadioGroup` does, with step-by-step examples. Suppose our `dataProvider` has an endpoint that returns languages as follows.

```ts title="https://refine-fake-rest.pankod.com/languages"
{
    [
        {
            id: 1,
            title: "Turkish",
        },
        {
            id: 2,
            title: "English",
        },
        {
            id: 3,
            title: "German",
        },
    ];
}
```

```tsx title="src/pages/posts/create.tsx"
import { Form, Radio, useRadioGroup } from "@pankod/refine";

export const PostCreate = () => {
    //highlight-start
    const { radioGroupProps } = useRadioGroup({
        resource: "languages",
    });
    //highlight-end

    return (
        <Form>
            ...
            <Form.Item label="Languages" name="languages">
                //highlight-next-line
                <Radio.Group {...radioGroupProps} />
            </Form.Item>
            ...
        <Form>
    );
};
```

<div>
    <img src={basicUsage} />
</div>
<br/>

All we have to do is pass the `radioGroupProps` it returns to the `<Radio.Group>` component.

`useRadioGroup` uses the `useList` hook for fetching data. [Refer to Ant Design `useList` hook for details. &#8594](data/useList.md)

## Options

### `resource`

```tsx
const { radioGroupProps } = useRadioGroup({
    //highlight-next-line
    resource: "languages",
});
```

`resource` property determines API resource endpoint to fetch records from data provider. It returns properly configured `options` values for radio buttons.

[Refer to Ant Design `Radio.Group` component documentation for detailed info for `options`. &#8594](https://ant.design/components/radio)

### `optionLabel` and `optionValue`

```tsx
const { radioGroupProps } = useRadioGroup({
    resource: "languages",
    //highlight-start
    optionLabel: "title",
    optionValue: "id",
    //highlight-end
});
```

Allows you to change the values and appearance of your options. Default values are `optionLabel = "title"` and `optionValue = "id"`.

### `filters`

```tsx
const { radioGroupProps } = useRadioGroup({
    resource: "languages",
    //highlight-next-line
    filters: { isActive: true },
});
```

It allows us to add some filters while fetching the data. For example, if you want to list only active records.

### `sort`

```tsx
const { radioGroupProps } = useRadioGroup({
    resource: "languages",
    //highlight-start
    sort: [
        {
            field: "title",
            order: "asc",
        },
    ],
    //highlight-end
});
```

It allows us to sort the `options`. For example, if you want to sort your list according to `title` by ascending.

## Add Type

```tsx title="src/pages/posts/create.tsx"
import { Form, useRadioGroup } from "@pankod/refine";
//highlight-next-line
import { ILanguage } from "interfaces";

export const PostCreate = () => {
    //highlight-next-line
    const { queryResult } = useRadioGroup<ILanguage>({
        resource: "languages",
    });
};
```

```ts title="interfaces/index.d.ts"
export interface ILanguage {
    id: string;
    title: string;
}
```

<br/>

Now, we expect the `queryResult` result to return according to `ILanguage` type.

## API Reference

### Properties

| Property                                          | Description                               | Type     | Default   |
| ------------------------------------------------- | ----------------------------------------- | -------- | --------- |
| resource <div className="required">Required</div> | [`Resource`](#) for API data interactions | `string` |           |
| optionValue                                       | Set the option's value                    | `string` | `"id"`    |
| optionLabel                                       | Set the option's label value              | `string` | `"title"` |
| filters                                           | Add filters while fetching the data       | ``       |           |
| sort                                              | Allow us to sort the options              | ``       |           |

### Return values

| Property        | Description                     | Type                                                                            |
| --------------- | ------------------------------- | ------------------------------------------------------------------------------- |
| radioGroupProps | Ant design radio group props    | [`Radio Group`](https://ant.design/components/radio/#RadioGroup)                |
| queryResult     | Result of the query of a record | [`QueryObserverResult<T>`](https://react-query.tanstack.com/reference/useQuery) |