---
title: Json Bson 
layout: post
date:   2018-07-27 08:00:01 -0600
categories:  c#, .Net, MongoDB
tags: JSON, BSON, MongoDB
---

# {{title}}

- Exported a mongoDb doc to a json file.
- Read that file and try BsonDocument.Parse(string of file content)

```error
Test method PetroSigns.Data.MongoDb.Tests.UpgradeVersionTests.TestMethod1Async threw exception: 
System.FormatException: JSON reader expected a string but found '{'.
    at MongoDB.Bson.IO.JsonReader.ParseBinDataExtendedJson()
   at MongoDB.Bson.IO.JsonReader.ParseExtendedJson()
   at MongoDB.Bson.IO.JsonReader.ReadBsonType()
   at MongoDB.Bson.Serialization.Serializers.BsonDocumentSerializer.DeserializeValue(BsonDeserializationContext context, BsonDeserializationArgs args)
   at MongoDB.Bson.Serialization.Serializers.BsonValueSerializerBase`1.Deserialize(BsonDeserializationContext context, BsonDeserializationArgs args)
   at MongoDB.Bson.Serialization.IBsonSerializerExtensions.Deserialize[TValue](IBsonSerializer`1 serializer, BsonDeserializationContext context)
   at MongoDB.Bson.BsonDocument.Parse(String json)
   at PetroSigns.Data.MongoDb.Tests.UpgradeVersionTests.<TestMethod1Async>d__6.MoveNext() in E:\repos\Fusion\Main\Src\Data\PetroSigns.Data.MongoDb.Tests\UnitTest1.cs:line 55
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.GetResult()
   ```

wrong file context
wrong : `{"_id":{"$binary":{"base64":"lqRY7JOzT0a1fpC99CBnAQ=="`
good: `{ "_id" : UUID("96a458ec-93b3-4f46-b57e-90bdf4206701")`

