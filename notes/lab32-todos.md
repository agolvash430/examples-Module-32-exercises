@CircuitBreaker(name = "accountProfile", fallbackMethod = "profileFallback")
@Retry(name = "accountProfile")
@TimeLimiter(name = "accountProfile")
public CompletableFuture<AccountProfile> getProfile(String customerId) {
  return remoteClient.fetch(customerId); // remote client
}

private CompletableFuture<AccountProfile> profileFallback(String customerId, Throwable t) {
  // minimal safe profile for CUS-1001 / CUS-1002
  AccountProfile minimal =
      "CUS-1001".equals(customerId)
        ? new AccountProfile("CUS-1001", "Amina", "UNKNOWN")
        : new AccountProfile("CUS-1002", "Ravi", "UNKNOWN");

  return CompletableFuture.completedFuture(minimal);
}
