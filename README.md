SlimCachingManager
==================

If you want to do dynamic caching of slim resources feel free to use the **SlimCachingManager**. You can inject resources (+ lifetime) into the *ResourceMapper* which should be cached.

The data of the given resources is being stored by individual *ResourceHandlers* (Database, textfile etc.). You can write your own handlers by implementing the *IResourceHandler* Interface. You can find two examples for *ResourceHandlers* in the "Ezd" directory.
The *ResourceHandler* provides methods for read() and write() data (e.g. etag, expiry date and lifetime) for the current resource. It's also possible to implement a garbage collector which will remove the cache if the resource is out of date.

Just follow the steps at the bottom of the page ("Usage"). **SlimCachingManager** will set etags and expiry headers on his own.

If you have any questions or suggestions to improve **SlimCachingManager** feel free to ask or fork the repo.

Best regards
Magnus

Usage
--------
1. Create instance of Slim
2. Add resources to the ResourceMapper which should be cached (Wildcards allowed!)
3. Create instance of Slim\Http\Caching\ResourceMapper();
4. Inject your own ResourceHandler into ResourceMapper
5. Inject the Slim application into ResourceMapper
6. Call method setHeaders()

Example
--------
	<?php
	
	# Create Slim instance
	$app = new \Slim\Slim();
	
	# Add resource '/fetch/my/resource/' to my cache list
	Slim\Http\Caching\ResourceMapper::addResourceWildcard( '/fetch/my/resource/', 24 );
	
	# If you want to cache more than one resource
	Slim\Http\Caching\ResourceMapper::addResourceWildcard(
		Array(
			'/fetch/my/resource/' => 24,
			'/fetch/my/second/resource/' => 5,
			'/fetch/products/list/' => 1
		)
	);
	
	# Create instance of the ResourceMapper
	$cachingManager = new Slim\Http\Caching\ResourceMapper();
	
	# Inject Handler and Slim Application and set Headers
    $cachingManager->setHandler( new Ezd\Caching\ResourceHandler( $db ) )
        ->setApplication( $app )
        ->setHeaders();

	?>
	
That's it. ETag and Expires will be set automatically.

Have fun!