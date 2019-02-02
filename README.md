Use in your project by creating a logger.module.ts with content like this:

```javascript
import { Module } from "@nestjs/common";
import { ConfigModule } from "../config/config.module";
import { LoggerService } from "nestlogger";
import { ConfigService } from "../config/config.service";

@Module({
  imports: [ConfigModule],
  providers: [
    {
      provide: LoggerService,
      useFactory: (config: ConfigService) => {
        return new LoggerService(config.logLevel, config.serviceName, config.logAppenders, config.logFilePath);
      },
      inject: [ConfigService],
    },
  ],
  exports: [LoggerService],
})
export class LoggerModule {}
```

Then import logger module wherever you need it:

```javascript
...
import { LoggerModule } from "../logging/logger.module";

@Module({
  imports: [
    LoggerModule,
    DBModule,
    ElasticSearchModule,
    HttpModule,
  ],
  controllers: [ItemController],
  providers: [ItemService],
})
export class ItemModule {}
```

And log stuff:
```javascript
this.logger.debug(`Found ${result.rowCount} items from db`, ItemService.name);
this.logger.error(`Error while getting items from db`, err.stack, ItemService.name);
```
