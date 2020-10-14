
# utils.c

- `rand()` : 0 ~ 32767 사이의 랜덤한 값을 반환합니다.

## what_time_is_it_now

```c
double what_time_is_it_now()
{
    struct timeval time;
    if (gettimeofday(&time,NULL)){
        return 0;
    }
    return (double)time.tv_sec + (double)time.tv_usec * .000001;
}
```

- 현재 시간을 초단위로 구합니다.

## read_intlist

```c
int *read_intlist(char *gpu_list, int *ngpus, int d)
{
    int *gpus = 0;
    if(gpu_list){
        int len = strlen(gpu_list);
        *ngpus = 1;
        int i;
        for(i = 0; i < len; ++i){
            if (gpu_list[i] == ',') ++*ngpus;
        }
        gpus = calloc(*ngpus, sizeof(int));
        for(i = 0; i < *ngpus; ++i){
            gpus[i] = atoi(gpu_list);
            gpu_list = strchr(gpu_list, ',')+1;
        }
    } else {
        gpus = calloc(1, sizeof(float));
        *gpus = d;
        *ngpus = 1;
    }
    return gpus;
}
```

## read_map

```c
int *read_map(char *filename)
{
    int n = 0;
    int *map = 0;
    char *str;
    FILE *file = fopen(filename, "r");
    if(!file) file_error(filename);
    while((str=fgetl(file))){
        ++n;
        map = realloc(map, n*sizeof(int));
        map[n-1] = atoi(str);
    }
    return map;
}
```

- 파일의 내용을 int형으로 mapping하여 반환합니다.

## sorta_shuffle

```c
void sorta_shuffle(void *arr, size_t n, size_t size, size_t sections)
{
    size_t i;
    for(i = 0; i < sections; ++i){
        size_t start = n*i/sections;
        size_t end = n*(i+1)/sections;
        size_t num = end-start;
        shuffle(arr+(start*size), num, size);
    }
}
```

- ???

## shuffle

```c
void shuffle(void *arr, size_t n, size_t size)
{
    size_t i;
    void *swp = calloc(1, size);
    for(i = 0; i < n-1; ++i){
        size_t j = i + rand()/(RAND_MAX / (n-i)+1);
        memcpy(swp,          arr+(j*size), size);
        memcpy(arr+(j*size), arr+(i*size), size);
        memcpy(arr+(i*size), swp,          size);
    }
}
```

- array를 shuffle 합니다.

## random_index_order

```c
int *random_index_order(int min, int max)
{
    int *inds = calloc(max-min, sizeof(int));
    int i;
    for(i = min; i < max; ++i){
        inds[i] = i;
    }
    for(i = min; i < max-1; ++i){
        int swap = inds[i];
        int index = i + rand()%(max-i);
        inds[i] = inds[index];
        inds[index] = swap;
    }
    return inds;
}
```

- min ~ max사이의 index를 shuffle 합니다.

## del_arg

```c
void del_arg(int argc, char **argv, int index)
{
    int i;
    for(i = index; i < argc-1; ++i) argv[i] = argv[i+1];
    argv[i] = 0;
}
```

- index번째 argument를 제거 합니다.

## find_arg

```c
int find_arg(int argc, char* argv[], char *arg)
{
    int i;
    for(i = 0; i < argc; ++i) {
        if(!argv[i]) continue;
        if(0==strcmp(argv[i], arg)) {
            del_arg(argc, argv, i);
            return 1;
        }
    }
    return 0;
}
```

- arguments에 값이 있는 경우 1 없는 경우 0을 반환합니다.

## find_int_arg

```c
int find_int_arg(int argc, char **argv, char *arg, int def)
{
    int i;
    for(i = 0; i < argc-1; ++i){
        if(!argv[i]) continue;
        if(0==strcmp(argv[i], arg)){
            def = atoi(argv[i+1]);
            del_arg(argc, argv, i);
            del_arg(argc, argv, i);
            break;
        }
    }
    return def;
}
```

- argument에 해당하는 int형 인자값을 반환합니다.

## find_float_arg

```c
float find_float_arg(int argc, char **argv, char *arg, float def)
{
    int i;
    for(i = 0; i < argc-1; ++i){
        if(!argv[i]) continue;
        if(0==strcmp(argv[i], arg)){
            def = atof(argv[i+1]);
            del_arg(argc, argv, i);
            del_arg(argc, argv, i);
            break;
        }
    }
    return def;
}
```

- argument에 해당하는 float형 인자값을 반환합니다.

## find_char_arg

```c
char *find_char_arg(int argc, char **argv, char *arg, char *def)
{
    int i;
    for(i = 0; i < argc-1; ++i){
        if(!argv[i]) continue;
        if(0==strcmp(argv[i], arg)){
            def = argv[i+1];
            del_arg(argc, argv, i);
            del_arg(argc, argv, i);
            break;
        }
    }
    return def;
}
```

- argument에 해당하는 char형 인자값을 반환합니다.

## basecfg

```c
char *basecfg(char *cfgfile)
{
    char *c = cfgfile;
    char *next;
    while((next = strchr(c, '/')))
    {
        c = next+1;
    }
    c = copy_string(c);
    next = strchr(c, '.');
    if (next) *next = 0;
    return c;
}
```

- 파일 경로에서 cfg 파일의 이름을 반환 합니다.

- 예를 들어 `../cfg/yolo.cfg`인 경우 `yolo`를 반환합니다.

## alphanum_to_int

```c
int alphanum_to_int(char c)
{
    return (c < 58) ? c - 48 : c-87;
}
```

- 문자를 숫자로 바꿉니다.

## int_to_alphanum

```c
char int_to_alphanum(int i)
{
    if (i == 36) return '.';
    return (i < 10) ? i + 48 : i + 87;
}
```

- 숫자를 문자로 바꿉니다.

## pm

```c
void pm(int M, int N, float *A)
{
    int i,j;
    for(i =0 ; i < M; ++i){
        printf("%d ", i+1);
        for(j = 0; j < N; ++j){
            printf("%2.4f, ", A[i*N+j]);
        }
        printf("\n");
    }
    printf("\n");
}
```

- M x N 크기를 가지는 A행렬을 출력합니다.

## find_replace

```c
void find_replace(char *str, char *orig, char *rep, char *output)
{
    char buffer[4096] = {0};
    char *p;

    sprintf(buffer, "%s", str);
    if(!(p = strstr(buffer, orig))){  // Is 'orig' even in 'str'?
        sprintf(output, "%s", str);
        return;
    }

    *p = '\0';

    sprintf(output, "%s%s%s", buffer, rep, p+strlen(orig));
}
```

- `strstr` : 문자열안에 특정 문자(열)이 있는지 확인 해주는 함수
- `str` 문자열에서 `orig` 문자열을 찾아 `rep`로 변경한 뒤 `output`에 저장하는 함수

## sec

```c
float sec(clock_t clocks)
{
    return (float)clocks/CLOCKS_PER_SEC;
}
```

- 코드 실행시간을 측정하기 위한 함수 입니다.

- 소비된 시간 clock_t(clock ticks) 자료형을 가지는 clocks에 CLOCKS_PER_SEC(초당 클럭수)을 나누어 주어야 합니다.

## top_k

```c
void top_k(float *a, int n, int k, int *index)
{
    int i,j;
    for(j = 0; j < k; ++j) index[j] = -1;
    for(i = 0; i < n; ++i){
        int curr = i;
        for(j = 0; j < k; ++j){
            if((index[j] < 0) || a[curr] > a[index[j]]){
                int swap = curr;
                curr = index[j];
                index[j] = swap;
            }
        }
    }
}
```

- 배열에서 값이 큰 순서대로 k개를 뽑아서 index를 반환합니다.

## error

```c
void error(const char *s)
{
    perror(s);
    assert(0);
    exit(-1);
}
```

- `perror` : 오류 메세지를 출력합니다.

## read_file

```c
unsigned char *read_file(char *filename)
{
    FILE *fp = fopen(filename, "rb");
    size_t size;

    fseek(fp, 0, SEEK_END);
    size = ftell(fp);
    fseek(fp, 0, SEEK_SET);

    unsigned char *text = calloc(size+1, sizeof(char));
    fread(text, 1, size, fp);
    fclose(fp);
    return text;
}
```

- 파일을 읽어 반환합니다.

## malloc_error

```c
void malloc_error()
{
    fprintf(stderr, "Malloc error\n");
    exit(-1);
}
```

- 메모리 할당 에러

## file_error

```c
void file_error(char *s)
{
    fprintf(stderr, "Couldn't open file: %s\n", s);
    exit(0);
}

```

- file을 읽는데 에러가 발생하는 경우를 처리합니다.

## split_str

```c
list *split_str(char *s, char delim)
{
    size_t i;
    size_t len = strlen(s);
    list *l = make_list();
    list_insert(l, s);
    for(i = 0; i < len; ++i){
        if(s[i] == delim){
            s[i] = '\0';
            list_insert(l, &(s[i+1]));
        }
    }
    return l;
}
```

- delim을 기준으로 문자열 s를 split해서 list에 담아 반환합니다.

## strip


```c
void strip(char *s)
{
    size_t i;
    size_t len = strlen(s);
    size_t offset = 0;
    for(i = 0; i < len; ++i){
        char c = s[i];
        if(c==' '||c=='\t'||c=='\n') ++offset;
        else s[i-offset] = c;
    }
    s[len-offset] = '\0';
}
```

- 문자열에 포함되어있는 공백을 제거해줍니다.

## strip_char

```c
void strip_char(char *s, char bad)
{
    size_t i;
    size_t len = strlen(s);
    size_t offset = 0;
    for(i = 0; i < len; ++i){
        char c = s[i];
        if(c==bad) ++offset;
        else s[i-offset] = c;
    }
    s[len-offset] = '\0';
}
```

- 문자열에 포함되어있는 bad를 제거해줍니다.

## free_ptrs

```c
void free_ptrs(void **ptrs, int n)
{
    int i;
    for(i = 0; i < n; ++i) free(ptrs[i]);
    free(ptrs);
}
```

- 포인터 메모리 할당을 해제합니다.

## fgetl

```c
char *fgetl(FILE *fp)
{
    if(feof(fp)) return 0;
    size_t size = 512;
    char *line = malloc(size*sizeof(char));
    if(!fgets(line, size, fp)){
        free(line);
        return 0;
    }

    size_t curr = strlen(line);

    while((line[curr-1] != '\n') && !feof(fp)){
        if(curr == size-1){
            size *= 2;
            line = realloc(line, size*sizeof(char));
            if(!line) {
                printf("%ld\n", size);
                malloc_error();
            }
        }
        size_t readsize = size-curr;
        if(readsize > INT_MAX) readsize = INT_MAX-1;
        fgets(&line[curr], readsize, fp);
        curr = strlen(line);
    }
    if(line[curr-1] == '\n') line[curr-1] = '\0';

    return line;
}
```

- 파일에서 한줄을 읽어 반환합니다.

## read_int

```c
int read_int(int fd)
{
    int n = 0;
    int next = read(fd, &n, sizeof(int));
    if(next <= 0) return -1;
    return n;
}
```

- 파일에 int형 값을 읽어서 n에 입력합니다.

## write_int

```c
void write_int(int fd, int n)
{
    int next = write(fd, &n, sizeof(int));
    if(next <= 0) error("read failed");
}
```

- 파일에 int형 값 n을 입력합니다.

## read_all_fail

```c
int read_all_fail(int fd, char *buffer, size_t bytes)
{
    size_t n = 0;
    while(n < bytes){
        int next = read(fd, buffer + n, bytes-n);
        if(next <= 0) return 1;
        n += next;
    }
    return 0;
}
```

- read_all과 같은 기능
- 읽기에서 오류나는 경우 1을 반환 아닌 경우 0을 반환합니다.

## write_all_fail

```c
int write_all_fail(int fd, char *buffer, size_t bytes)
{
    size_t n = 0;
    while(n < bytes){
        size_t next = write(fd, buffer + n, bytes-n);
        if(next <= 0) return 1;
        n += next;
    }
    return 0;
}
```

- write_all과 같은 기능
- 쓰기에서 오류나는 경우 1을 반환 아닌 경우 0을 반환합니다.

## read_all

```c
void read_all(int fd, char *buffer, size_t bytes)
{
    size_t n = 0;
    while(n < bytes){
        int next = read(fd, buffer + n, bytes-n);
        if(next <= 0) error("read failed");
        n += next;
    }
}
```

- 파일에 내용을 buffer에 읽어들입니다.

## write_all

```c
void write_all(int fd, char *buffer, size_t bytes)
{
    size_t n = 0;
    while(n < bytes){
        size_t next = write(fd, buffer + n, bytes-n);
        if(next <= 0) error("write failed");
        n += next;
    }
}
```

- 파일에 buffer에 담긴 내용을 입력합니다.

## copy_string

```c
char *copy_string(char *s)
{
    char *copy = malloc(strlen(s)+1);
    strncpy(copy, s, strlen(s)+1);
    return copy;
}
```

- 문자열을 복사하여 반환합니다.

## parse_csv_line

```c
list *parse_csv_line(char *line)
{
    list *l = make_list();
    char *c, *p;
    int in = 0;
    for(c = line, p = line; *c != '\0'; ++c){
        if(*c == '"') in = !in;
        else if(*c == ',' && !in){
            *c = '\0';
            list_insert(l, copy_string(p));
            p = c+1;
        }
    }
    list_insert(l, copy_string(p));
    return l;
}
```

- 문장하나를 입력받아 list에 넣어 반환합니다.

## count_fields

```c
int count_fields(char *line)
{
    int count = 0;
    int done = 0;
    char *c;
    for(c = line; !done; ++c){
        done = (*c == '\0');
        if(*c == ',' || done) ++count;
    }
    return count;
}
```

- `,`를 기준으로 분리하여 개수를 반환합니다.

## parse_fields

```c
float *parse_fields(char *line, int n)
{
    float *field = calloc(n, sizeof(float));
    char *c, *p, *end;
    int count = 0;
    int done = 0;
    for(c = line, p = line; !done; ++c){
        done = (*c == '\0');
        if(*c == ',' || done){
            *c = '\0';
            field[count] = strtod(p, &end);
            if(p == c) field[count] = nan("");
            if(end != c && (end != c-1 || *end != '\r')) field[count] = nan(""); //DOS file formats!
            p = c+1;
            ++count;
        }
    }
    return field;
}
```

- `strtod` : 문자열을 실수 값으로 반환합니다.

- 한줄을 읽어들여 배열 값을 반환합니다.

## sum_array

```c
float sum_array(float *a, int n)
{
    int i;
    float sum = 0;
    for(i = 0; i < n; ++i) sum += a[i];
    return sum;
}
```

- 배열 a의 합을 반환합니다.

## mean_array

```c
float mean_array(float *a, int n)
{
    return sum_array(a,n)/n;
}
```

- 배열 a의 평균을 반환합니다.

## mean_arrays

```c
void mean_arrays(float **a, int n, int els, float *avg)
{
    int i;
    int j;
    memset(avg, 0, els*sizeof(float));
    for(j = 0; j < n; ++j){
        for(i = 0; i < els; ++i){
            avg[i] += a[j][i];
        }
    }
    for(i = 0; i < els; ++i){
        avg[i] /= n;
    }
}
```

- 2차원 배열 a의 평균을 반환합니다.

## print_statistics

```c
void print_statistics(float *a, int n)
{
    float m = mean_array(a, n);
    float v = variance_array(a, n);
    printf("MSE: %.6f, Mean: %.6f, Variance: %.6f\n", mse_array(a, n), m, v);
}
```

- mse, mean, variance를 출력합니다.

## variance_array

```c
float variance_array(float *a, int n)
{
    int i;
    float sum = 0;
    float mean = mean_array(a, n);
    for(i = 0; i < n; ++i) sum += (a[i] - mean)*(a[i]-mean);
    float variance = sum/n;
    return variance;
}
```

- 배열 a의 분산을 반환합니다.

## constrain_int

```c
int constrain_int(int a, int min, int max)
{
    if (a < min) return min;
    if (a > max) return max;
    return a;
}
```

- int
- min보다 작으면 min
- max보다 크면 max
- 항상 min ~ max의 값만 반환한다.

## constrain

```c
float constrain(float min, float max, float a)
{
    if (a < min) return min;
    if (a > max) return max;
    return a;
}
```

- float
- min보다 작으면 min
- max보다 크면 max
- 항상 min ~ max의 값만 반환한다.

## dist_array

```c
float dist_array(float *a, float *b, int n, int sub)
{
    int i;
    float sum = 0;
    for(i = 0; i < n; i += sub) sum += pow(a[i]-b[i], 2);
    return sqrt(sum);
}
```

- a와 b의 유클리드 거리를 구합니다.
- $$\sqrt{\sum_{i=1}^{n} (a_i - b_i)^2}$$

## mse_array

```c
float mse_array(float *a, int n)
{
    int i;
    float sum = 0;
    for(i = 0; i < n; ++i) sum += a[i]*a[i];
    return sqrt(sum/n);
}
```

- mean square를 구합니다.
- $$\frac{1}{N} \sum_{i=1}^{n} a_i^2$$

## normalize_array

```c
void normalize_array(float *a, int n)
{
    int i;
    float mu = mean_array(a,n);
    float sigma = sqrt(variance_array(a,n));
    for(i = 0; i < n; ++i){
        a[i] = (a[i] - mu)/sigma;
    }
    mu = mean_array(a,n);
    sigma = sqrt(variance_array(a,n));
}
```

- 배열 a를 정규화 합니다.

## translate_array

```c
void translate_array(float *a, int n, float s)
{
    int i;
    for(i = 0; i < n; ++i){
        a[i] += s;
    }
}
```

- 배열 a에 추가할 값을 더합니다.

## mag_array

```c
float mag_array(float *a, int n)
{
    int i;
    float sum = 0;
    for(i = 0; i < n; ++i){
        sum += a[i]*a[i];   
    }
    return sqrt(sum);
}
```

- $$\sqrt{\sum_{i=0}^{n} a_i^2}$$

## scale_array

```c
void scale_array(float *a, int n, float s)
{
    int i;
    for(i = 0; i < n; ++i){
        a[i] *= s;
    }
}
```

- 배열 a의 scale을 조절합니다.

## sample_array

```c
int sample_array(float *a, int n)
{
    float sum = sum_array(a, n);
    scale_array(a, n, 1./sum);
    float r = rand_uniform(0, 1);
    int i;
    for(i = 0; i < n; ++i){
        r = r - a[i];
        if (r <= 0) return i;
    }
    return n-1;
}
```

- ???

## max_int_index

```c
int max_int_index(int *a, int n)
{
    if(n <= 0) return -1;
    int i, max_i = 0;
    int max = a[0];
    for(i = 1; i < n; ++i){
        if(a[i] > max){
            max = a[i];
            max_i = i;
        }
    }
    return max_i;
}
```

- int 배열에서 최댓값 index를 반환합니다.

## max_index

```c
int max_index(float *a, int n)
{
    if(n <= 0) return -1;
    int i, max_i = 0;
    float max = a[0];
    for(i = 1; i < n; ++i){
        if(a[i] > max){
            max = a[i];
            max_i = i;
        }
    }
    return max_i;
}
```

- float 배열에서 최댓값 index를 반환합니다.

## int_index

```c
int int_index(int *a, int val, int n)
{
    int i;
    for(i = 0; i < n; ++i){
        if(a[i] == val) return i;
    }
    return -1;
}
```

- 배열 a에서 val의 index를 반환합니다.

## rand_int

```c
int rand_int(int min, int max)
{
    if (max < min){
        int s = min;
        min = max;
        max = s;
    }
    int r = (rand()%(max - min + 1)) + min;
    return r;
}
```

- min ~ max 사이의 정수를 반환합니다.

## rand_normal

```c
// From http://en.wikipedia.org/wiki/Box%E2%80%93Muller_transform
float rand_normal()
{
    static int haveSpare = 0;
    static double rand1, rand2;

    if(haveSpare)
    {
        haveSpare = 0;
        return sqrt(rand1) * sin(rand2);
    }

    haveSpare = 1;

    rand1 = rand() / ((double) RAND_MAX);
    if(rand1 < 1e-100) rand1 = 1e-100;
    rand1 = -2 * log(rand1);
    rand2 = (rand() / ((double) RAND_MAX)) * TWO_PI;

    return sqrt(rand1) * cos(rand2);
}
```

- [http://en.wikipedia.org/wiki/Box%E2%80%93Muller_transform](http://en.wikipedia.org/wiki/Box%E2%80%93Muller_transform)

## rand_size_t

```c
size_t rand_size_t()
{
    return  ((size_t)(rand()&0xff) << 56) |
        ((size_t)(rand()&0xff) << 48) |
        ((size_t)(rand()&0xff) << 40) |
        ((size_t)(rand()&0xff) << 32) |
        ((size_t)(rand()&0xff) << 24) |
        ((size_t)(rand()&0xff) << 16) |
        ((size_t)(rand()&0xff) << 8) |
        ((size_t)(rand()&0xff) << 0);
}
```

- 8 byte 크기의 랜덤한 값을 생성합니다.

## rand_uniform

```c
float rand_uniform(float min, float max)
{
    if(max < min){
        float swap = min;
        min = max;
        max = swap;
    }
    return ((float)rand()/RAND_MAX * (max - min)) + min;
}
```

- max와 min사이의 랜덤한 값을 반환합니다.

## rand_scale

```c
float rand_scale(float s)
{
    float scale = rand_uniform(1, s);
    if(rand()%2) return scale;
    return 1./scale;
}
```

- 1에서 s사이의 랜덤한 값을 반환합니다.

## one_hot_encode

```c
float **one_hot_encode(float *a, int n, int k)
{
    int i;
    float **t = calloc(n, sizeof(float*));
    for(i = 0; i < n; ++i){
        t[i] = calloc(k, sizeof(float));
        int index = (int)a[i];
        t[i][index] = 1;
    }
    return t;
}
```

- 크기가 n인 배열 a를 one hot encoding 합니다.

```
a : [0, 1, 2]

return [[1, 0, 0], [0, 1, 0], [0, 0, 1]]
```

## sum_array

```c
float sum_array(float *a, int n)
{
    int i;
    float sum = 0;
    for(i = 0; i < n; ++i) sum += a[i];
    return sum;
}
```

- 배열의 총합을 구합니다.


## mean_array

```c
float mean_array(float *a, int n)
{
    return sum_array(a,n)/n;
}
```

- 배열의 평균을 구합니다.
