# IParking 8

## Generic
### Conventions

The base URL to send all API requests is  `http://192.168.21.231`.

The API follows RESTful conventions when possible,The API follows RESTful conventions whenever possible, and all requests must be authenticated before use.

Property names are in  <span style="color:red">camelCase</span>.

### Status Codes

HTTP response codes are used to indicate general classes of success and error. If the request is processed successfully, the response will return `200 OK` or `201 Created`.

If an error occurs, the response will return `400 Bad Request` along with error details in the response body. In cases where the response returns `404 Not Found`, it may indicate that the requested object does not exist in the system.

Error responses contain more details about the error in the response body, including the `errorCode` and `detailCode` properties. Additionally, the `success` property will be set to false.

The response will return `400 Bad Request` with detailed error information in the response body. The error details will be included in the `fields` property, which contains:

- `name:` The name of the field or object that caused the error.

- `errorCode`: The specific error code describing the issue.

`errorCode`:

| Parameter | Description         | 
| :-------- |------------------------- |
| `ERROR.ENTITY.NOT_FOUND` | The entity does not exist in the system.  | 
| `ERROR.ENTITY.VALIDATION.FIELD_INVALID`|The submitted field is invalid.|
| `ERROR.ENTITY.VALIDATION.FIELD_DUPLICATED`|The submitted field already exists in the system.|
| `ERROR.ENTITY.VALIDATION.FIELD_NOT_ACTIVE`| The entity is not yet active. |
|`ERROR.ENTITY.NOT_FOUND.SOME_ITEMS_DELETED`| Some entities do not exist in the system.|
|`ERROR.ENTITY.VALIDATION.FIELD_REQUIRED`| Required. |

### Object

- `AccessKeyCollection` : Nhóm định danh

| Parameter | Type  | Description                |
| :-------- | :-------|:------------------------- |
| `id`|||
| `name`   |  `string`  | Tên của nhóm định danh. |
| `code`   |  `string`  | Mã của nhóm định danh. |
| `vehicle-type`   |  `VehicleType`  |  Loại phương tiện. |
| `enabled`| `bool`| Trạng thái, mặc định là true |
| `attributes`|`array<AccesskeyAttribute>`| Các thuộc tính của nhóm định danh.| 

- `AccesskeyAttribute`

| Parameter | Type  | Description                |
| :-------- | :-------|:------------------------- |
| `name`   |  `string`  | Tên của thuộc tính. |
| `value`   |  `string`  | Giá trị  |

- `AccessKey`: Định danh

| Parameter | Type  | Description                |
| :-------- | :-------|:------------------------- |
| `id` | `uuid` | Id của định danh. |
| `name`   |  `string`  | Tên của định danh. |
| `code`   |  `string`  | Mã của định danh. |
| `collection`   |  `AccessKeyCollection`| Nhóm định danh. |
| `status` | `AccessKeyStatus`| Trạng thái. |
| `type` | `AccessKeyType`| Loại định danh.| 
| `note` | `string` | Ghi chú.| 

- `Device`: Thiết bị 

| Parameter | Type  | Description                |
| :-------- | :-------|:------------------------- |
| `id` | `uuid` | Id của thiết bị. |
| `name`   |  `string`  | Tên thiết bị. |
| `code`   |  `string`  | Mã thiết bị. |
| `type`   |  `DeviceType`| Loại thiết bị. |
| `enabled` | `bool`| Trạng thái. |
| `parent` | `Device`| Thiết bị.| 

### Enum

- `AccessKeyStatus`: Trạng thái của định danh, 0: Locked, 1: Use, 2: UnUse
- `VehicleType`: Loại phương tiện, 0 : Car , 1 : MotorBike, 2: Bike
- `AccessKeyType`: Loại định danh, 0: Vehicle, 1: Card, 2: QrCode
- `DeviceType`: Loại thiết bị, 0: Gate, 1: Computer, 2: Camera, 3: ControlUnit, 4: Lane, 5: Led 

## API Reference

### Access Key Collection

#### Search

```http
POST /access-key-collections/search
``` 

| Parameter | Type  
| :-------- | :-------|
| `pageIndex`   |  `int`  |
| `pageSize`   |  `int`  |
#### Add

```http
POST /access-key-collections
```

| Parameter | Type  |  Required | Description                |
| :-------- | :-------| :------- | :------------------------- |
| `name`   |  `string`  | `Yes`  | Tên của nhóm định danh. |
| `code`   |  `string`  | `Yes`  | Mã của nhóm định danh. |
| `vehicle-type`   |  `VehicleType`  | `Yes`  | Loại phương tiện. |
| `enabled`| `bool`| `No` | Trạng thái, mặc định là true |
| `attributes`|`array<object>`| `No` | các thuộc tính của nhóm định danh.| 

#### Update

```http
PUT /access-key-collections/{id}
```

| Parameter | Type  |  Required | Description                |
| :-------- | :-------| :------- | :------------------------- |
| `name`   |  `string`  | `Yes`  | Tên của nhóm định danh. |
| `code`   |  `string`  | `Yes`  | Mã của nhóm định danh. |
| `vehicle-type`   |  `VehicleType`  | `Yes`  | Loại phương tiện. |
| `enabled`| `bool`| `No` | Trạng thái, mặc định là true |
| `attributes`|`array<object>`| `No` | các thuộc tính của nhóm định danh.|

#### Get by id

```http
GET /access-key-collections/{id}
``` 
#### Delete

```http
DELETE /access-key-collections/{id}
``` 
### Access Key

#### Search

```http
POST /access-keys/search
``` 

| Parameter | Type  
| :-------- | :-------|
| `pageIndex`   |  `int`  |
| `pageSize`   |  `int`  |
#### Add

```http
POST /access-keys
```

| Parameter | Type  |  Required | Description                |
| :-------- | :-------| :------- | :------------------------- |
| `name`   |  `string`  | `Yes`  | Tên của định danh. |
| `code`   |  `string`  | `Yes`  | Mã của định danh. |
| `collectionid`   |  `Uuid`  | `Yes`  | Id của nhóm định danh. |
| `status` | `AccessKeyStatus`| `Yes` | Trạng thái. |
| `type` | `AccessKeyType`|`Yes` | Loại định danh.| 
| `note` | `string` |`No` |Ghi chú.| 

#### Update

```http
PUT /access-keys/{id}
```

 Parameter | Type  |  Required | Description                |
| :-------- | :-------| :------- | :------------------------- |
| `name`   |  `string`  | `Yes`  | Tên của định danh. |
| `code`   |  `string`  | `Yes`  | Mã của định danh. |
| `collectionid`   |  `Uuid`  | `Yes`  | Id của nhóm định danh. |
| `status` | `AccessKeyStatus`| `Yes` | Trạng thái. |
| `type` | `AccessKeyType`|`Yes` | Loại định danh.| 
| `note` | `string` |`No` |Ghi chú.| 

#### Get by id

```http
GET /access-keys/{id}
``` 
#### Delete

```http
DELETE /access-keys/{id}
``` 
