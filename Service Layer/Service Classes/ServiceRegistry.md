class ServiceRegistry:
    _services = {}

    @classmethod
    def register(cls, service_name):
        def decorator(service_class):
            cls._services[service_name] = service_class
            return service_class
        return decorator

    @classmethod
    def get_service(cls, service_name):
        return cls._services.get(service_name)

    @classmethod
    def list_services(cls):
        return list(cls._services.keys())

# Example usage
service_registry = ServiceRegistry()

@service_registry.register("user_service")
class UserService:
    def __init__(self):
        pass
    def get_user(self, user_id):
        return f"User with ID {user_id}"

@service_registry.register("product_service")
class ProductService:
    def __init__(self):
        pass
    def get_product(self, product_id):
         return f"Product with ID {product_id}"

# Retrieve and use services
user_service = service_registry.get_service("user_service")
if user_service:
    print(user_service().get_user(123))

product_service = service_registry.get_service("product_service")
if product_service:
     print(product_service().get_product(456))

# List all registered services
print("Registered services:", service_registry.list_services())
