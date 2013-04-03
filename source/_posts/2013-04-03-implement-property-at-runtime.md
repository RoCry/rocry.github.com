---
layout: post
title: "Implement the @dynamic @property at runtime"
date: 2013-04-03 18:30
comments: true
categories: [Runtime, Cocoa, Objective-C, Note]
---
In some cases, you need to overwrite the property's origin setter/getter methods(Maybe), or you even don't want to synthesize those. At the moment you could use objc's runtime to do this...

At First, you need to get all the @dynamic property at runtime
{% codeblock lang:objc %}
    Class objectClass = xxx;
    
    unsigned int numOfProperties, pi;
    objc_property_t *properties = class_copyPropertyList(objectClass, &numOfProperties);
    for (pi = 0; pi < numOfProperties; pi++) {
        objc_property_t property = properties[pi];
        const char * name = property_getName(property);
        // name like this:
        // thePropertyName
        const char * attrs = property_getAttributes(property);
        // attrs like this :
        // T@"NSString",C,D
        // T for type
        // C for copy
        // D for Dynamic
        // and the doc:
        // https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtPropertyIntrospection.html#//apple_ref/doc/uid/TP40008048-CH101-SW6
    }
    free(properties);
{% endcodeblock %}
At now you got the property name and the attribute of it, you could implement the setter like this:
{% codeblock lang:objc %}
// @
id getter(id self, SEL _cmd)
{
    return THE_REAL_GETTER;
}

void setter(id self, SEL _cmd, id value)
{
    [THE_REAL_SETTER:value];
}

+ (void)synthesizeWithGetterName:(NSString *)getterName {
    // the chars "@@:" is like property's attributes above, more details:
    // https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html#//apple_ref/doc/uid/TP40008048-CH100-SW1
    class_addMethod([self class], NSSelectorFromString(getterName),
                (IMP)getter, "@@:");                
    class_addMethod([self class], NSSelectorFromString(setterName),
                    (IMP)setter, "v@:@");
}
{% endcodeblock %}
So far you have implement some basic setter/getter, because in REAL WORLD, you must handle the different cases such as copy/weak/types besides id...   

At last, runtime is fucking powerful!!