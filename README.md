# Front-End Project Guidelines

### Framework

- Next JS 13

### Code Editor

- Visual Studio Code

#### VScode Setting

```json
"typescript.preferences.importModuleSpecifier": "relative",
"editor.codeActionsOnSave": {
  "source.organizeImports": true,
},
"editor.formatOnSave": true,
```

#### Extension

- Prettier
- SonarLint
- Turbo Console Log

### Naming Conventions

| Name        | Example      |
| ----------- | ------------ |
| Camel Case  | `camelCase`  |
| Snake Case  | `snake_case` |
| Pascal Case | `PascalCase` |

### File naming

| Asset Type | Suffix                                 | Example                                            |
| ---------- | -------------------------------------- | -------------------------------------------------- |
| Page       | `Home, React Icon`                     | `home.tsx, react-icon.tsx`                         |
| Component  | `Button`                               | `Button.tsx`                                       |
| Use Cases  | `Get Albums`                           | `getAlbum.usecase.ts`                              |
| Repository | `Album Repository`                     | `album.repository.ts`                              |
| Response   | `albumRes`                             | `albumRes.json`                                    |
| Unit Test  | `Get Albums`                           | `getAlbum.usecase.test.ts`                         |
| Spy        | `Get Albums Spy, Album Repository Spy` | `getAlbum.usecase.spy.ts, album.repository.spy.ts` |

### Image naming

| Asset Type | Prefix | Example                                                        |
| ---------- | ------ | -------------------------------------------------------------- |
| Icons      | `ic_`  | `ic_like.png, ic_like_active.png, ic_like_circle, ic_like_red` |
| Background | `bg_`  | `bg_worldcup_campaign.png`                                     |
| Button     | `btn_` | `btn_search_red.png, btn_search_active.png`                    |

### Directory Structure

```
├── public
│   ├── images
│   │   ├── icons
│   │   │   └── ic_like.png
│   │   └── background
│   │   │   └── bg_campaign.png
│   ├── fonts
│   │   └── roboto.ttf
├── components
│   ├── button
│   │   ├── types
│   │   ├── Button.module.scss
│   │   ├── Button.tsx
│   │   └── button.viewmodel.ts
├── pages
│   ├── api
│   │   └── users.ts
│   ├── home
│   │   ├── layout.tsx
│   │   ├── loading.tsx
│   │   └── page.tsx
│   ├── layout.tsx
│   └── page.tsx
├── src
│   ├── adapters
│   │   └── storage.adapter.ts
│   ├── constants
│   │   └── httpStatus.ts
│   ├── viewmodel
│   │   ├── home-page
│   │   │   ├── types
│   │   │   └── homePage.viewmodel.ts
│   ├── repositories
│   │   ├── album
│   │   │   ├── types
│   │   │   └── album.repository.ts
│   ├── usecases
│   │   └── album
│   │   │   ├── types
│   │   │   └── getAlbum.usecase.ts
│   ├── utils
│   │   ├── genSlugId.ts
│   │   └── formatDate.ts
│   ├── i18n
│   │   ├── types
│   │   │   ├── i18n.ts
│   │   │   └── locales.ts
│   │   ├── locales
│   │   │   ├── en.ts
│   │   │   └── th.ts
│   │   └── i18nLocale.ts
├── __tests__
│   ├── res
│   │   ├── album.res.json
│   │   └── albums.res.json
│   ├── spy
│   │   ├── repositories
│   │   │   ├── album.repository.spy.ts
│   │   │   └── index.ts
│   │   └── usecases
│   │   │   ├── getAlbums.usecase.spy.ts
│   │   │   └── index.ts
│   ├── repositories
│   │   └── album.repository.test.ts
│   ├── usecases
│   │   └── getAlbum.usecase.test.ts
│   └── viewmodel
│   │   └── albums.viewmodel.test.ts
├── .env
└── .env.local

```

## Variables

### ประกาศ const ไว้ที่เดียวกัน จากนั้นตามด้วยการประกาศ let ไว้ที่เดียวกัน (อย่าสลับไปมา) และใส่ตัวแปรที่ยังไม่ได้กำหนดค่าไว้ด้านล่างเสมอ

> ไม่ควรใช้ var เพราะว่า let และ const จะมีค่าอยู่แค่ในปีกกาที่ประกาศ (Block-scoped) เท่านั้น จึงปลอดภัย

```javascript
// ไม่ดี
let i,
  len,
  dragonball,
  items = [],
  isSports = true;

// ไม่ดี
let i;
const items = [];
let dragonball;
const isSports = true;
let len;

// ดี
const isSports = true; // ประกาศตัวแปร type boolean ให้ขึ้นต้นด้วย is เสมอ
const items = [];
let dragonball;
let i;
let length;
```

## Access requirements

| Asset Type | Level |
| ---------- | ----- |
| ViewModel  | 1     |
| Usecase    | 2     |
| Repository | 3     |

- **ViewModel** - ทำหน้าที่เป็นตัวกลางในการสื่อสารระหว่าง View (UI) และ Model โดย ViewModel จะถือข้อมูลและสถานะที่จำเป็นสำหรับการแสดงผลใน UI และจัดการกับการกระทำที่เกิดขึ้นใน UI **_Access Level 2 ได้เท่านั้น จะไม่สามารถ Access Level 3 ได้_**
- **Usecase** - ประกอบด้วย application-specific business logic หรือ กรณีการใช้งานแต่ละกรณีแสดงถึงการกระทำหรือฟังก์ชันเฉพาะของแอปพลิเคชันของคุณ **_Access Level 2 และ Level 3 ได้เท่านั้น ไม่สามารถ Access Level 1 ได้_**
- **Repository** - จัดการการเข้าถึงข้อมูลและการโต้ตอบกับฐานข้อมูล และการใช้งานแหล่งข้อมูลจากส่วนอื่นๆของแอปพลิเคชัน

## Function prototype name

### View

```JSX
interface Props {}

const HomePage = (props: Props) => {
  const viewModel = homePageViewModel();

  useEffect(() => {
    viewModel.getAlbums();
  }, []);

  return (
    <div>
    {viewModel.album && (
      <div className={`${styles.album} ${styles.borderBottom}`}>
      <p>
        {viewModel.album.id} {viewModel.album.title}
      </p>
      <p className={styles.album}>User ID: {viewModel.album.userId}</p>
      </div>
    )}
    {viewModel.albums.map((album, idx) => (
      <div
        className={`${styles.pointer} ${styles.borderBottom}`}
        key={idx}
        onClick={() => viewModel.getAlbum(album.id)}
      >
      <p>{album.title}</p>
      <p>{album.userId}</p>
      </div>
    ))}
    </div>
  );
};

export default HomePage;
```

### ViewModel

```typescript
export const homePageViewModel = () => {
  const [album, setAlbum] = useState<Album>(null);
  const [albums, setAlbums] = useState<Album[]>([]);

  const getAlbum = async (id: number) => {
    const result = await getAlbumUseCase(id);
    setAlbum(result);
  };

  const getAlbums = async () => {
    const result = await getAlbumsUseCase();
    setAlbums(result);
  };

  return {
    album,
    albums,
    getAlbum,
    getAlbums,
  };
};
```

### Usecase

```typescript
import {
  ApplicationRepository,
  ApplicationRepositoryImpl,
} from "../../repositories/application/application.repository";
import { ApplicationRecord } from "../../repositories/application/types/applicationRecord";
import { CreateApplicationRecord } from "../../repositories/application/types/createApplicationRecord";

export interface CreateApplicationUsecase {
  execute(payload: CreateApplicationRecord): Promise<ApplicationRecord>;
}

export class CreateApplicationUsecaseImpl implements CreateApplicationUsecase {
  private applicationRepository: ApplicationRepository;

  constructor(
    applicationRepository: ApplicationRepository = new ApplicationRepositoryImpl()
  ) {
    this.applicationRepository = applicationRepository;
  }

  async execute(payload: CreateApplicationRecord): Promise<ApplicationRecord> {
    const recode = await this.applicationRepository.createApplication(payload);
    return recode;
  }
}
```

### Repository

```typescript
import { pb } from "../../pocketbase/pb";
import { CollectionName } from "../types/collection";
import { ApplicationRecord } from "./types/applicationRecord";
import { CreateApplicationRecord } from "./types/createApplicationRecord";

export interface ApplicationRepository {
  createApplication(data: CreateApplicationRecord): Promise<ApplicationRecord>;
}

export class ApplicationRepositoryImpl implements ApplicationRepository {
  async createApplication(
    data: CreateApplicationRecord
  ): Promise<ApplicationRecord> {
    const record = await pb
      .collection(CollectionName.Application)
      .create<ApplicationRecord>(data);
    return record;
  }
}
```

## Unit Testing

### Test Matcher

```
toEqual() : เทียบแบบ == [ใช้เปรียบเทียบค่า obj / ตัวเลข]

toBe() : เทียบแบบ ===

toMatch : เทียบ string

toThrow() : เช็คว่าโปรแกรม throw error (Exception)

toHaveBeenCalledWith(): เมื่อใช้กับ mock function จะตรวจสอบว่าฟังก์ชันถูกเรียกใช้งานด้วยอาร์กิวเมนต์ที่กำหนดให้ตรงกับอาร์กิวเมนต์ที่ระบุหรือไม่ ตัวอย่างการใช้งาน

toHaveBeenCalledTimes(): เมื่อใช้กับ mock function จะตรวจสอบว่าฟังก์ชันถูกเรียกใช้งานจำนวนครั้งที่กำหนดหรือไม่ ตัวอย่างการใช้งาน
```

### Mock Functions Spy

```typescript
// RepositorySpy
import { ApplicationRepository } from "../../../repositories/application/application.repository";

export const applicationRepositorySpy: jest.Mocked<ApplicationRepository> = {
  createApplication: jest.fn(),
};
```

```typescript
// UsecaseSpy
import { CreateApplicationUsecase } from "../../../usecases/application/createApplication.usecase";

export const createApplicationUsecaseSpy: jest.Mocked<CreateApplicationUsecase> =
  {
    execute: jest.fn(),
  };
```

### Test UseCase

```typescript
import { ApplicationRepository } from "../../../repositories/application/application.repository";
import { ApplicationRecord } from "../../../repositories/application/types/applicationRecord";
import { CreateApplicationRecord } from "../../../repositories/application/types/createApplicationRecord";
import {
  CreateApplicationUsecase,
  CreateApplicationUsecaseImpl,
} from "../../../usecases/application/createApplication.usecase";
import { applicationRepositorySpy } from "../../spy/repositories/application.repository.spy";

describe("CreateApplicationUsecaseImpl", () => {
  let usecase: CreateApplicationUsecase;
  let applicationRepositoryMock: jest.Mocked<ApplicationRepository>;

  beforeEach(() => {
    applicationRepositoryMock = applicationRepositorySpy;

    usecase = new CreateApplicationUsecaseImpl(applicationRepositoryMock);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  test("execute_should_application", async () => {
    // Given
    const payloadMock: CreateApplicationRecord = {
      user: "",
      loan_coverage: 0,
      loan_interest_rate: 0,
      loan_installment_month: 0,
      loan_duration_year: 0,
      verified: false,
      otp: "",
      income: "",
      debt: "",
      costomer_info: "",
      product_type: "",
      brand: "",
      car_model: "",
      car_model_image: "",
      year_model: "",
      signature: "",
      bundle: "",
      bundle_coverage: 0,
      bundle_duration_year: 0,
      my_case: "",
      status: "",
      score: 0,
    };
    const applicationRecordMock: ApplicationRecord = {
      id: "",
      collectionId: "",
      collectionName: "",
      created: "",
      updated: "",
      user: "",
      loan_coverage: 0,
      loan_interest_rate: 0,
      loan_installment_month: 0,
      loan_duration_year: 0,
      verified: false,
      otp: "",
      income: undefined,
      debt: undefined,
      costomer_info: undefined,
      product_type: "",
      brand: undefined,
      car_model: undefined,
      car_model_image: undefined,
      year_model: undefined,
      signature: "",
      bundle: undefined,
      bundle_coverage: 0,
      bundle_duration_year: 0,
      my_case: "",
      status: "Success",
      score: 0,
      watched: false,
    };

    applicationRepositoryMock.createApplication.mockResolvedValueOnce(
      applicationRecordMock
    );

    // When
    const result = await usecase.execute(payloadMock);

    // Then
    expect(result).toEqual(applicationRecordMock);
    expect(applicationRepositoryMock.createApplication).toHaveBeenCalledWith(
      payloadMock
    );
    expect(applicationRepositoryMock.createApplication).toHaveBeenCalledTimes(
      1
    );
  });

  test("execute_should_error", async () => {
    // Given
    const payloadMock: CreateApplicationRecord = {
      user: "",
      loan_coverage: 0,
      loan_interest_rate: 0,
      loan_installment_month: 0,
      loan_duration_year: 0,
      verified: false,
      otp: "",
      income: "",
      debt: "",
      costomer_info: "",
      product_type: "",
      brand: "",
      car_model: "",
      car_model_image: "",
      year_model: "",
      signature: "",
      bundle: "",
      bundle_coverage: 0,
      bundle_duration_year: 0,
      my_case: "",
      status: "",
      score: 0,
    };
    const errorMock = new Error("Async error Create Application");

    applicationRepositoryMock.createApplication.mockRejectedValueOnce(
      errorMock
    );

    try {
      // When
      await usecase.execute(payloadMock);
    } catch (error) {
      // Then
      expect(applicationRepositoryMock.createApplication).toHaveBeenCalledWith(
        payloadMock
      );
      expect(applicationRepositoryMock.createApplication).toHaveBeenCalledTimes(
        1
      );
      expect(error).toBe(errorMock);
    }
  });
});
```

### Test Repository

```typescript
import { ListResult, RecordQueryParams } from "pocketbase";
import { pb } from "../../../pocketbase/pb";
import {
  ApplicationRepository,
  ApplicationRepositoryImpl,
} from "../../../repositories/application/application.repository";
import { ApplicationRecord } from "../../../repositories/application/types/applicationRecord";
import { CreateApplicationRecord } from "../../../repositories/application/types/createApplicationRecord";
import { CollectionName } from "../../../repositories/types/collection";

describe("ApplicationRepositoryImpl", () => {
  let repository: ApplicationRepository;
  let pocketbaseSpy: jest.SpyInstance;
  let recordMock: ApplicationRecord;

  beforeEach(() => {
    recordMock = {
      id: "",
      collectionId: "",
      collectionName: "",
      created: "",
      updated: "",
      user: "",
      loan_coverage: 0,
      loan_interest_rate: 0,
      loan_installment_month: 0,
      loan_duration_year: 0,
      verified: false,
      otp: "",
      income: undefined,
      debt: undefined,
      costomer_info: undefined,
      product_type: "",
      brand: undefined,
      car_model: undefined,
      car_model_image: undefined,
      year_model: undefined,
      signature: "",
      bundle: undefined,
      bundle_coverage: 0,
      bundle_duration_year: 0,
      my_case: "",
      status: "Success",
      score: 0,
      watched: false,
    };
    repository = new ApplicationRepositoryImpl();
  });

  afterEach(() => {
    pocketbaseSpy.mockRestore();
  });

  test("createApplication_should_createRecord", async () => {
    // Given
    const createMock: CreateApplicationRecord = {
      user: "",
      loan_coverage: 0,
      loan_interest_rate: 0,
      loan_installment_month: 0,
      loan_duration_year: 0,
      verified: false,
      otp: "",
      income: "",
      debt: "",
      costomer_info: "",
      product_type: "",
      brand: "",
      car_model: "",
      car_model_image: "",
      year_model: "",
      signature: "",
      bundle: "",
      bundle_coverage: 0,
      bundle_duration_year: 0,
      my_case: "",
      status: "",
      score: 0,
    };

    pocketbaseSpy = jest.spyOn(
      pb.collection(CollectionName.Application),
      "create"
    );
    pocketbaseSpy.mockResolvedValueOnce(recordMock);

    // When
    const result = await repository.createApplication(createMock);

    // Then
    expect(result).toEqual(recordMock);
    expect(pocketbaseSpy).toHaveBeenCalledWith(createMock);
    expect(pocketbaseSpy).toHaveBeenCalledTimes(1);
  });

  test("createApplication_should_error", async () => {
    // Given
    const createMock: CreateApplicationRecord = {
      user: "",
      loan_coverage: 0,
      loan_interest_rate: 0,
      loan_installment_month: 0,
      loan_duration_year: 0,
      verified: false,
      otp: "",
      income: "",
      debt: "",
      costomer_info: "",
      product_type: "",
      brand: "",
      car_model: "",
      car_model_image: "",
      year_model: "",
      signature: "",
      bundle: "",
      bundle_coverage: 0,
      bundle_duration_year: 0,
      my_case: "",
      status: "",
      score: 0,
    };
    const errorRecord = new Error("Async error");

    pocketbaseSpy = jest.spyOn(
      pb.collection(CollectionName.Application),
      "create"
    );
    pocketbaseSpy.mockRejectedValueOnce(errorRecord);

    // When
    try {
      await repository.createApplication(createMock);
    } catch (error) {
      // Then
      expect(pocketbaseSpy).toHaveBeenCalledTimes(1);
      expect(error).toBe(errorRecord);
    }
  });

  test("getOneApplication_should_applicationRecord", async () => {
    // Given
    const idMock = "oe3wl123";
    const queryParamsMock: RecordQueryParams = {
      expand:
        "costomer_info, brand, car_model, car_model_image, year_model, income, debt, product_type, bundle",
    };

    pocketbaseSpy = jest.spyOn(
      pb.collection(CollectionName.Application),
      "getOne"
    );
    pocketbaseSpy.mockResolvedValueOnce(recordMock);

    // When
    const result = await repository.getOneApplication(idMock);

    // Then
    expect(result).toEqual(recordMock);
    expect(pocketbaseSpy).toHaveBeenCalledWith(idMock, queryParamsMock);
    expect(pocketbaseSpy).toHaveBeenCalledTimes(1);
  });

  test("getListApplication_should_listApplicationRecord", async () => {
    // Given
    const pageMock = 1;
    const limitMock = 20;
    const listResultMock: ListResult<ApplicationRecord> = {
      page: pageMock,
      perPage: limitMock,
      totalItems: 0,
      totalPages: 0,
      items: [recordMock],
    };
    const queryParamsMock: RecordQueryParams = {
      filter: `my_case = '' && delete = ''`,
      expand: "costomer_info, car_model, car_model_image, product_type",
    };

    pocketbaseSpy = jest.spyOn(
      pb.collection(CollectionName.Application),
      "getList"
    );
    pocketbaseSpy.mockResolvedValueOnce(listResultMock);

    // When
    const result = await repository.getListApplication(pageMock, limitMock);

    // Then
    expect(result).toEqual(listResultMock);
    expect(pocketbaseSpy).toHaveBeenCalledWith(
      pageMock,
      limitMock,
      queryParamsMock
    );
    expect(pocketbaseSpy).toHaveBeenCalledTimes(1);
  });
});
```

### Test ViewModel

```typescript
import { renderHook, act, waitFor } from "@testing-library/react";

// ข้อระวัง ต้องเป็น File Path UseCase ตัวจริง ไม่ใช่ File Path Spy
jest.mock("../getAlbumUseCase"); // File Path UseCase
jest.mock("../getAlbumsUseCase"); // File Path UseCase

const sut = () => {
  return renderHook(() => useAlbumsViewModel());
};

describe("AlbumsViewModel", () => {
  afterEach(() => {
    albumUseCaseSpy.destroy();
    albumsUseCaseSpy.destroy();
  });

  test("Initial album => Should return null", () => {
    // When
    const { result } = sut();

    // Then
    expect(result.current.album).toBe(null);
  });

  test("Get Album By ID => Should return album", async () => {
    // Given
    const { result } = sut();
    albumUseCaseSpy.init(albumRes);

    // When
    act(() => {
      result.current.getAlbum(1);
    });

    // Then
    await waitFor(() => {
      expect(result.current.album).toEqual(albumRes); // expect แล้วแต่ละกรณีตาม logic
    });
  });
});
```
