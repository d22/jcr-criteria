[![Build Status](https://travis-ci.org/vpro/jcr-criteria.svg?)](https://travis-ci.org/vpro/jcr-criteria)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/nl.vpro/jcr-criteria/badge.svg?style=plastic)](https://maven-badges.herokuapp.com/maven-central/nl.vpro/jcr-criteria)

# jcr-criteria

This is a way to create and execute JCR queries using Java code, using an interface which was inspired by the Criteria API as used by Hibernate/JPA. This code is based on the [Criteria API for Magnolia CMS](http://www.openmindlab.com/lab/products/mgnlcriteria.html) (openutils-mgnlcriteria) module which was developed by Openmind.

In contrast to `openutils-mgnlcriteria` there is no dependency on any Magnolia CMS code in `jcr-criteria`, so this is a generic JCR Criteria API implementation. It can still be used with Magnolia CMS, but should work with other JCR-based projects as well.

## Usage

You can download the most recent jar from https://oss.sonatype.org/content/repositories/snapshots/nl/vpro/jcr-criteria/ or you can add this to your `pom.xml`:

```xml
<dependency>
    <groupId>nl.vpro</groupId>
    <artifactId>jcr-criteria</artifactId>
    <version>1.2</version>
</dependency>
```

Example:

```java
import nl.vpro.jcr.criteria.query.AdvancedResult;
import nl.vpro.jcr.criteria.query.AdvancedResultItem;
import nl.vpro.jcr.criteria.query.Criteria;
import nl.vpro.jcr.criteria.query.JCRCriteriaFactory;
import nl.vpro.jcr.criteria.query.criterion.Criterion;
import nl.vpro.jcr.criteria.query.criterion.Restrictions;

..

Criteria criteria = JCRCriteriaFactory.createCriteria()
            .setBasePath(basePath)
            .setPaging(1, 1)
            .addOrder(Order.ascending(field))
            .add(Restrictions.eq(Criterion.JCR_PRIMARYTYPE, NodeTypes.Page.NAME))
            .add(Restrictions.in("@" + NodeTypes.Renderable.TEMPLATE, templates))
            .add(Restrictions.gt(field, begin));

 AdvancedResult ar = criteria.execute(MgnlContext.getJCRSession(RepositoryConstants.WEBSITE));
 LOG.debug("JCR query : " + criteria.toXpathExpression());
 AdvancedResultItem item = ar.getFirstResult();
```
