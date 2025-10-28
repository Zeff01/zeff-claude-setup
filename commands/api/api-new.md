# Create New API Endpoint (RepairCoin)

Create a new API endpoint for RepairCoin. This command supports both:
1. **Backend Express API** (Domain-Driven Design) - Primary use case
2. **Frontend Next.js API Routes** - For frontend-only features

---

## Part 1: Backend Express API (Primary)

Use this for main backend features requiring database access, blockchain interaction, or cross-domain events.

### Architecture

RepairCoin uses Domain-Driven Design with these domains:
- **admin** - Platform analytics, treasury, user management
- **customer** - Customer management, tiers, referrals, balances
- **shop** - Shop subscriptions, RCN purchasing, reward issuance
- **token** - RCN/RCG minting, redemption, cross-shop transfers
- **webhook** - FixFlow and Stripe webhook processing

### Directory Structure

```
backend/src/domains/{domain}/
├── {Domain}Domain.ts           # Domain module
├── routes/
│   ├── index.ts               # Main router
│   └── {feature}.ts           # Feature routes
├── controllers/
│   └── {Feature}Controller.ts
└── services/
    └── {Feature}Service.ts
```

### Step 1: Create Routes File

**Path**: `backend/src/domains/{domain}/routes/{feature}.ts`

```typescript
import { Router } from 'express';
import { requireRole, authMiddleware } from '../../../middleware/auth';
import {
  validateRequired,
  validateEthereumAddress,
  validateEmail,
  validateNumeric,
  asyncHandler
} from '../../../middleware/errorHandler';
import { {Feature}Controller } from '../controllers/{Feature}Controller';
import { {Feature}Service } from '../services/{Feature}Service';

const router = Router();

// Initialize service and controller
const {feature}Service = new {Feature}Service();
const {feature}Controller = new {Feature}Controller({feature}Service);

/**
 * @swagger
 * /api/{domain}/{feature}:
 *   post:
 *     summary: {Description}
 *     tags: [{Domain}]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required:
 *               - field1
 *             properties:
 *               field1:
 *                 type: string
 *     responses:
 *       200:
 *         description: Success
 */
router.post('/{feature}',
  authMiddleware,
  requireRole(['admin', 'shop']),
  validateRequired(['field1']),
  asyncHandler({feature}Controller.{method}.bind({feature}Controller))
);

export default router;
```

### Step 2: Create Controller

**Path**: `backend/src/domains/{domain}/controllers/{Feature}Controller.ts`

```typescript
import { Request, Response } from 'express';
import { {Feature}Service } from '../services/{Feature}Service';
import { ResponseHelper } from '../../../utils/responseHelper';

export class {Feature}Controller {
  constructor(private {feature}Service: {Feature}Service) {}

  async {method}(req: Request, res: Response) {
    try {
      const { field1, field2 } = req.body;
      const { id } = req.params;

      const result = await this.{feature}Service.{method}({
        field1,
        field2,
        id,
        userAddress: req.user?.address,
        userRole: req.user?.role
      });

      ResponseHelper.success(res, result);
    } catch (error: unknown) {
      const err = error as Error;
      if (err.message === 'Not found') {
        ResponseHelper.notFound(res, err.message);
      } else if (err.message === 'Unauthorized') {
        ResponseHelper.forbidden(res, err.message);
      } else if (err.message.includes('already exists')) {
        ResponseHelper.conflict(res, err.message);
      } else {
        ResponseHelper.error(res, err.message, 500);
      }
    }
  }
}
```

### Step 3: Create Service

**Path**: `backend/src/domains/{domain}/services/{Feature}Service.ts`

```typescript
import { logger } from '../../../utils/logger';
import { {Entity}Repository } from '../../../repositories/{Entity}Repository';
import { eventBus, createDomainEvent } from '../../../events/EventBus';

interface {Method}Input {
  field1: string;
  field2?: number;
  userAddress?: string;
  userRole?: string;
}

export class {Feature}Service {
  private {entity}Repository: {Entity}Repository;

  constructor() {
    this.{entity}Repository = new {Entity}Repository();
  }

  async {method}(input: {Method}Input): Promise<unknown> {
    logger.info('{Feature}Service.{method} called', { input });

    try {
      // Validation
      if (!input.field1) {
        throw new Error('field1 is required');
      }

      // Authorization
      if (input.userRole !== 'admin') {
        throw new Error('Unauthorized');
      }

      // Business logic
      const result = await this.{entity}Repository.{operation}({
        field1: input.field1
      });

      if (!result) {
        throw new Error('Not found');
      }

      // Publish event
      await eventBus.publish(createDomainEvent(
        '{domain}.{event}',
        result.id,
        { field1: input.field1 },
        '{Feature}Service'
      ));

      logger.info('{Feature}Service.{method} completed');

      return {
        success: true,
        data: result
      };
    } catch (error: unknown) {
      const err = error as Error;
      logger.error('{Feature}Service.{method} error', {
        error: err.message
      });
      throw error;
    }
  }
}
```

### Step 4: Register Routes

Update `backend/src/domains/{domain}/routes/index.ts`:

```typescript
import { Router } from 'express';
import {feature}Routes from './{feature}';

const router = Router();

router.use('/{feature}', {feature}Routes);

export default router;
```

### Middleware Options

```typescript
// Authentication
authMiddleware                              // JWT auth

// Authorization
requireRole(['admin'])                      // Admin only
requireRole(['admin', 'shop'])              // Admin or Shop
requireRole(['customer'])                   // Customer only

// Validation
validateRequired(['field1', 'field2'])      // Required fields
validateEthereumAddress('walletAddress')    // Ethereum address
validateEmail('email')                      // Email format
validateNumeric('amount', 0.1, 1000)        // Numeric range
```

### Response Helpers

```typescript
ResponseHelper.success(res, data, 'Message');
ResponseHelper.created(res, data, 'Created');
ResponseHelper.badRequest(res, 'Validation error');
ResponseHelper.unauthorized(res, 'Auth required');
ResponseHelper.forbidden(res, 'Insufficient permissions');
ResponseHelper.notFound(res, 'Not found');
ResponseHelper.conflict(res, 'Already exists');
ResponseHelper.error(res, 'Internal error', 500);
```

### Testing

**Path**: `backend/tests/{domain}/{feature}.test.ts`

```typescript
import request from 'supertest';
import { describe, it, expect, beforeAll } from '@jest/globals';
import RepairCoinApp from '../../src/app';

describe('{Feature} API', () => {
  let app: unknown;
  let token: string;

  beforeAll(async () => {
    process.env.NODE_ENV = 'test';
    const repairCoinApp = new RepairCoinApp();
    await repairCoinApp.initialize();
    app = repairCoinApp.app;

    const auth = await request(app as any)
      .post('/api/auth/admin')
      .send({ walletAddress: process.env.ADMIN_ADDRESSES?.split(',')[0] });
    token = auth.body.token;
  });

  it('should create {feature}', async () => {
    const response = await request(app as any)
      .post('/api/{domain}/{feature}')
      .set('Authorization', `Bearer ${token}`)
      .send({ field1: 'value1' });

    expect(response.status).toBe(200);
    expect(response.body.success).toBe(true);
  });

  it('should reject unauthorized', async () => {
    const response = await request(app as any)
      .post('/api/{domain}/{feature}')
      .send({ field1: 'value1' });

    expect(response.status).toBe(401);
  });
});
```

### Checklist

- [ ] Identified correct domain
- [ ] Created routes file with middleware
- [ ] Created controller with error handling
- [ ] Created service with business logic
- [ ] Registered routes in domain index
- [ ] Added Swagger documentation
- [ ] Added TypeScript types (no `any`)
- [ ] Implemented auth/validation
- [ ] Published domain events if needed
- [ ] Created integration tests
- [ ] Tested with auth token

---

## Part 2: Frontend Next.js API Route (Secondary)

Use this for frontend-only features (e.g., client-side data transformation, SSR helpers).

### Directory Structure

```
frontend/app/api/{route}/route.ts
```

### Implementation

```typescript
import { NextRequest, NextResponse } from 'next';
import { z } from 'zod';

// Validation schema
const RequestSchema = z.object({
  field1: z.string().min(1),
  field2: z.number().optional()
});

type RequestBody = z.infer<typeof RequestSchema>;

// Response helper
function success<T>(data: T) {
  return NextResponse.json({ success: true, data });
}

function error(message: string, status: number = 500) {
  return NextResponse.json(
    { success: false, error: message },
    { status }
  );
}

export async function POST(req: NextRequest) {
  try {
    const body: unknown = await req.json();

    // Validate
    const validated = RequestSchema.parse(body) as RequestBody;

    // Business logic
    const result = {
      id: '123',
      ...validated
    };

    return success(result);
  } catch (err) {
    if (err instanceof z.ZodError) {
      return error(err.errors[0].message, 400);
    }
    return error('Internal error');
  }
}
```

---

## Important Notes

1. **Never use `any` type** - Always use proper TypeScript types
2. **Always use asyncHandler** - For Express route error handling
3. **Use ResponseHelper** - Consistent API responses
4. **Bind controller methods** - `.bind(controller)` in routes
5. **Validate inputs** - Use validation middleware
6. **Check authorization** - In service layer
7. **Log operations** - Use logger for debugging
8. **Publish events** - For cross-domain communication
9. **Write tests** - Integration tests with auth

## Examples

- Customer API: `backend/src/domains/customer/`
- Shop API: `backend/src/domains/shop/routes/subscription.ts`
- Token API: `backend/src/domains/token/routes/redemptionSession.ts`
