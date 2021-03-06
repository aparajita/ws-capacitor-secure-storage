# capacitor-secure-storage

This plugin for [Capacitor 2](https://capacitorjs.com) provides secure key/value storage on the web, iOS, and Android, with an API closely matching that of the Capacitor [Storage](https://capacitorjs.com/docs/apis/storage) plugin. If you are using the Storage plugin, this plugin is (more or less) a secure drop-in replacement.

👉 **NOTE:** This plugin has only been tested with Capacitor 2.

## Installation

```sh
pnpm install @aparajita/capacitor-secure-storage # 'pnpm add' also works
npm install @aparajita/capacitor-secure-storage
yarn add @aparajita/capacitor-secure-storage
```

Not using [pnpm](https://pnpm.js.org/)? You owe it to yourself to give it a try. It’s faster, better with monorepos, and uses *way, way* less disk space than the alternatives.

## Usage

The API is thoroughly documented [here](src/definitions.ts) and [below](#api). For a complete example of how to use this plugin in practice, see the [demo app](https://github.com/aparajita/capacitor-secure-storage-demo).

### Web

On the web, data is stored in `localStorage` by default. You may change that to `sessionStorage` by setting the `storageType` property.

Data is encrypted on the web using Blowfish encryption with no IV. Before modifying storage, you must call `setEncryptionKey` to set the “password” used to encrypt/decrypt the data.

### iOS

On iOS, data is stored in the encrypted system keychain and is specific to your app. Please note that currently iOS will **not** delete an app’s keychain data when the app is deleted. But since only an app with the same app id — which is guaranteed by Apple to be unique across all apps — can access that data, this is not a security issue.

### Android

On Android, data is encrypted using AES in GCM mode with a secret key generated by the Android KeyStore, then stored in SharedPreferences, which is specific to your app. If the app is deleted, its data is deleted as well.

## API

<docgen-index>

**Methods**<br>
[keys()](#keys)<br>
[setEncryptionKey(...)](#setencryptionkey)<br>
[set(...)](#set)<br>
[get(...)](#get)<br>
[remove(...)](#remove)<br>
[clear()](#clear)


[Enums](#enums)

</docgen-index>
<docgen-api>
<!--Update the source file JSDoc comments and rerun docgen to update the docs below-->

### keys()

```typescript
keys() => Promise<string[]>
```

Return a list of all keys with the current storage prefix.
The returned keys do not have the prefix.

**Returns:** Promise&lt;string[]&gt;

--------------------


### setEncryptionKey(...)

```typescript
setEncryptionKey(key: string) => void
```

web only

Set the secret key used to encrypt/decrypt data on the web,
using blowfish/ECB encryption without an IV.

If you are not using this plugin on the web (including testing),
then you do not need to call this method.

If you are using this plugin on the web, this method MUST be called
before set() or get().

If key is null or empty, StorageError(code: encryptionKeyNotSet)
is thrown.

| Param | Type   |
| :---- | :----- |
| key   | string |

--------------------


### set(...)

```typescript
set(key: string, data: DataType, convertDate?: boolean | undefined) => Promise<void>
```

Store data under a given key in the store. If data is not a string,
it is converted to stringified JSON first. If data is a Date and
convertDate is true (the default), it is converted to an ISO 8601 string
and stored as such. Note that dates within an object or an array
are converted to ISO strings by JSON.stringify, but will not be
converted back to dates by get().

On the web, if setEncryptionKey() has not been called successfully,
StorageError(code: encryptionKeyNotSet) is thrown.

| Param       | Type                                                                                                                   |
| :---------- | :--------------------------------------------------------------------------------------------------------------------- |
| key         | string                                                                                                                 |
| data        | \|&nbsp;string<br>\|&nbsp;number<br>\|&nbsp;boolean<br>\|&nbsp;Object<br>\|&nbsp;any[]<br>\|&nbsp;Date<br>\|&nbsp;null |
| convertDate | boolean                                                                                                                |

--------------------


### get(...)

```typescript
get(key: string, convertDate?: boolean | undefined) => Promise<DataType>
```

Retrieve data for a given key from the store. If the retrieved data
is in the form of an ISO 8601 date string and convertDate is true
(the default), it is converted to a Date.

If no item with the given key can be found, StorageError(code: notFound)
is thrown.

On the web, if setEncryptionKey() has not been called successfully,
StorageError(code: encryptionKeyNotSet) is thrown.

| Param       | Type    |
| :---------- | :------ |
| key         | string  |
| convertDate | boolean |

**Returns:** Promise&lt;DataType&gt;

--------------------


### remove(...)

```typescript
remove(key: string) => Promise<boolean>
```

Remove the data for a given key from the store.

| Param | Type   |
| :---- | :----- |
| key   | string |

**Returns:** Promise&lt;boolean&gt;

--------------------


### clear()

```typescript
clear() => Promise<void>
```

Remove all items from the store with the current key prefix.

--------------------


### Enums


#### StorageType

| Members        |
| :------------- |
| sessionStorage |
| localStorage   |

</docgen-api>
