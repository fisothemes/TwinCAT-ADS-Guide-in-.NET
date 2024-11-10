# Best Practices

+ **Minimise Processing in Handlers:** Keep event handlers efficient to avoid performance bottlenecks. Offload intensive tasks to a separate thread or process if needed.

+ **Unsubscribe Appropriately:** Always unsubscribe from events when they are no longer needed to prevent memory leaks.

+ **Ensure Thread Safety:** If your application is multi-threaded, ensure that event handlers are thread-safe and can handle concurrent updates properly.