# Handling Not Assignable Issues

## Problem

> Property 'config' of type 'ConfigType' is not assignable to 'string' index type 'string'.

The error here is generated by the following interfaces on the commented line.

```typescript
export interface ConfigType {
  type: string;
}

export interface DataModel {
  config: ConfigType; // ERROR HERE
  [key: string]: string;
}
```

The issue is that the `[key: string]: string;` line gets enforced on all key/value pairs on the interface.

## Type Approach

This approach seems much cleaner, creating a new data type that has the fixed and dynamic portions ANDed together.

```typescript
export interface ConfigType {
  type: string;
}

export type DataModel = {
  config: ConfigType;
} & {
  [key: string]: string;
};
```

## Union Approach

The following code is one resolution.

```typescript
export interface ConfigType {
  type: string;
}

export interface DataModel {
  config: ConfigType;
  [key: string]: string | ConfigType;
}
```

This approach has the "issue" that a new "key" could be used with another `ConfigType`.