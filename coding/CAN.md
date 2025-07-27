CAN����
============================
## һ�����
- CAN����(Controller Area Network Bus)��ȫ�ƿ��ƾ��������ߡ�  
- CAN������BOSCH��˾�з���һ��**�������**��**�����ٶȿ�**��**����չ**��**�ɿ��Ը�**��**����**ͨѶЭ�����ߡ�  

### 1. CAN��������
- **�����**���䣬**����������ǿ**  
- ����ͨ����(CAN_H,CAN_L)��**���蹲��**  
- ����CAN(ISO11898): 125k~1Mbps, <40m  
- ����CAN(ISO11519): 10k~125kbps, <1km  
- **�첽**��ͨѶ**����**ʱ���ߣ���ҪԼ��ͨѶ����  
- **��˫��**��֧��**���豸**��ͨ�������ٲ��ж�˳��  
- 11/29λ����ID(��׼/��չ),������Ϣ���������ȼ�  
- ����������8�ֽڵ���Ч�غ�  
- ��ʵ��**�㲥ʽ**��**����ʽ**���ִ��䷽ʽ  
- Ӧ��CRCУ�飬λ��䣬λͬ���������������

### 2. CANӲ����·
- CAN�豸ͨ��CAN�շ���������CAN������
- CAN������CAN������������TX��RX��CAN�շ���������CAN�շ���������CAN_H��CAN_L�ֱ������ߵ�CAN_H��CAN_L����
- ����CANʹ�ñջ����磬CAN_H��CAN_L�������120�����ն˵���![](CAN_img/����CAN.png "����CAN")
- ����CANʹ�ÿ������磬CAN_H��CAN_L����һ�����2.2k�����ն˵���![](CAN_img/����CAN.png "����CAN")


### 3. CAN��ƽ��׼
#### (1)����CAN
- ��ѹ��Ϊ0ʱ�����߼�1(���Ե�ƽ) ���޲���ʱ�����������ȵ�λ��    
- ��ѹ��Ϊ2Vʱ��ʾ�߼�0(���Ե�ƽ) ���豸�����������ͺţ�
#### (2)����CAN
- ��ѹ��Ϊ-1.5Vʱ�����߼�1(���Ե�ƽ)
- ��ѹ��Ϊ3Vʱ��ʾ�߼�0(���Ե�ƽ)


## ����֡��ʽ
  - CANЭ��涨��5�����͵�֡���ֱ�Ϊ
    - ����֡�������豸������������(�㲥ʽ)
    - ң��֡�������豸������������(����ʽ)
    - ����֡���豸���������������豸֪ͨ
    - ����֡�������豸��δ���ý���׼��
    - ֡��������ڸ�������֡��ң��֡������֡

### 1. ����֡
![](CAN_img/����֡.png "����֡")

### 2. ң��֡
- ң��֡�����ݶΣ�RTRΪ���Ե�ƽ1����������������֡��ͬ
![](CAN_img/ң��֡.png "ң��֡")

### 3. ����֡
- �����������豸����ල���ߵ����ݣ�һ�����֡�λ���󡱻������󡱻�CRC���󡱻򡰸�ʽ���󡱻�Ӧ����� ����Щ�豸��ᷢ������֡���ƻ����ݣ�ͬʱ��ֹ��ǰ�ķ����豸
![](CAN_img/����֡.png "����֡")

### 4. ����֡
- �����շ��յ��������ݶ��޷�����ʱ������Է�������֡���ӻ����ͷ������ݷ��ͣ���ƽ�����߸��أ��������ݶ�ʧ
![](CAN_img/����֡.png "����֡")

### 5. ֡���
- ������֡��ң��֡��ǰ���֡���뿪
![](CAN_img/֡���.png "֡���")

### 6. λ���
- ���ͷ�ÿ����5����ͬ��ƽ���Զ�׷��һ���෴��ƽ�����λ�����շ���⵽���λʱ���Զ��Ƴ��ָ�ԭʼ����
- ����
  - ���Ӳ��ζ�ʱ��Ϣ�����ڽ��շ�"��ͬ��"����ֹ���γ�ʱ���ޱ仯�����ܾ�ȷ���ղ���ʱ��
  - ����������������󡢹���֡��������־���󡢹���֡��������
  - ����CAN�����ڷ��������������Ļ�Ծ״̬����ֹ������Ϊ���߿���
















## STM32CAN����
### 1. ���
- STM32����bxCAN����(CAN������)��֧��CAN2.0A��2.0B,�����Զ�����CAN���ĺͰ��չ������Զ�����ָ��CAN���ģ�����ֻ��Ҫ���������ݶ������ע���ߵĵ�ƽϸ��
- ��������߿ɵ���**1Mbps**
- 3�����������ȼ��ķ�������
- 2��3����ȵĽ���FIFO Filter(�����ȳ�������)
- 14����������(������28��)
- �¼�����ͨ�š��Զ����߻ָ����Զ����ѡ���ֹ�Զ��ش�������FIFO�������ʽ�����á��������ȼ������á�˫CANģʽ

## FIFO������������

#### ��HAL���У�������ͨ��һ������ `HAL_CAN_ConfigFilter`�ĺ�������
#### �亯��ԭ������
```C
HAL_StatusTypeDef HAL_CAN_ConfigFilter(CAN_HandleTypeDef *hcan,
 const CAN_FilterTypeDef *sFilterConfig)
```
- ���нṹ��`CAN_HandleTypeDef`��HAL���ж����can���߽ṹ�壬ͨ��STM32CubeMx���ú���Զ�������Ϊ`hcan1`��`hcan2`��ʵ��  


���нṹ��`CAN_FilterTypeDef`��������
```C
    typedef struct
    {
    uint32_t FilterIdHigh;  //CAN�������ĸ�16λ��ʶ��
    uint32_t FilterIdLow;   //CAN�������ĵ�16λ��ʶ��
    uint32_t FilterMaskIdHigh;  //����CAN��������16λ����
    uint32_t FilterMaskIdLow;   //����CAN��������16λ����      
    uint32_t FilterFIFOAssignment;  //ʹ���ĸ�������
    uint32_t FilterBank;    //���������
    uint32_t FilterMode;    //������ģʽ
    uint32_t FilterScale;   //���������
    uint32_t FilterActivation;  //�Ƿ����ù�����
    uint32_t SlaveStartFilterBank;  //��CAN2����ʼ���������
    } CAN_FilterTypeDef;
```
- ����`FilterFIFOAssignment`ѡ����ƥ���ĸ�FIFO����ѡֵ��`CAN_RX_FIFO0`��`CAN_RX_FIFO1`���ֱ����FIFO0��FIFO1

- ����`FilterMode`��������ѡֵ��`CAN_FILTERMODE_IDMASK`��`CAN_FILTERMODE_IDLIST`���ֱ��ʾ����ģʽ���б�ģʽ
  - ����ģʽ��ͨ��ɸѡID�ض�λ�жϱ��ĵĽ����붪����**1λǿ��ƥ�䣬0λ����**
  - �б�ģʽ���г�ID�����һ������ܣ���֮������
  - `FilterScale`�����б���ID�������Ϊ32λ��ÿ����������д������ID����Ϊ16λ��ÿ����������д��4��ID

- ����`FilterScale`��������ѡֵ��`CAN_FILTERSCALE_16BIT`��`CAN_FILTERSCALE_32BIT`���ֱ��Ӧ16λ��Ⱥ�32λ��ȣ��������չ����ID��**����**ѡ��32λ

- ����`FilterActivation`��������ѡֵ��`CAN_FILTER_ENABLE`��`CAN_FILTER_DISABLE`���ֱ��ʾ�����͹ر�

- ����`SlaveStartFilterBank`Ӧ0-28��һ�������������ڶ�����������ʼλ
  
- һ������ʾ��
```C
void bsp_can_filter_config(CAN_HandleTypeDef *canHandle)
{
  CAN_FilterTypeDef filter = {0};
  filter.FilterActivation = ENABLE; //ʹ�ܹ�����
  filter.FilterMode = CAN_FILTERMODE_IDMASK;    //ʹ������ģʽ
  filter.FilterScale = CAN_FILTERSCALE_16BIT;   //16λ���
  filter.FilterBank = 0;    //ʹ��0�Ź�����
  filter.FilterFIFOAssignment = CAN_RX_FIFO0;   //ʹ��FIFO0
  filter.FilterIdLow = 0;   
  filter.FilterIdHigh = 0;
  filter.FilterMaskIdLow = 0;
  filter.FilterMaskIdHigh = 0;  //�����Ϊ0��
  HAL_CAN_ConfigFilter(canHandle, &filter);
}
```

## CAN����

## CAN����