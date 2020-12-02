# bitrix-components
```php
<?
if (CModule::IncludeModule('iblock')) {
    $arTypesEx = \CIBlockParameters::GetIBlockTypes(array('-' => ' '));
    $iBlockType = 'docs';
    if (!array_key_exists($iBlockType, $arTypesEx)) {
        $arSiteId = array();
        $rsSite = CSite::GetList($by = 'sort', $order = 'asc', array('ACTIVE' => 'Y'));
        while ($arSite = $rsSite->fetch()) {
            $arSiteId[] = $arSite['LID'];
        }

        $arNewTypeIBFields = array(
            'ID' => $iBlockType,
            'SECTIONS' => 'Y',
            'IN_RSS' => 'N',
            'SORT' => 500,
            'LANG' => array(
                'ru' => array(
                    'NAME' => 'Документы',
                    'SECTION_NAME' => '',
                    'ELEMENT_NAME' => ''
                ),
            )
        );
        $obBlocktype = new CIBlockType;
        $res = $obBlocktype->Add($arNewTypeIBFields);

        $ib = new CIBlock;
        $arNewIBFields = array(
            'ACTIVE' => 'Y',
            'NAME' => 'Докуметы',
            'CODE' => $iBlockType,
            'LIST_PAGE_URL' => '',
            'DETAIL_PAGE_URL' => '',
            'IBLOCK_TYPE_ID' => $iBlockType,
            'SITE_ID' => $arSiteId,
            'SORT' => '500',
            'VERSION' => '2',
            'GROUP_ID' => array('2' => 'R')
        );
        $idIblock = $ib->Add($arNewIBFields);

        if ($idIblock > 0) {
            $arFields = Array(
                "NAME" => "Файл",
                "ACTIVE" => "Y",
                "SORT" => "100",
                "CODE" => "FILE",
                "MULTIPLE" => "N",
                "PROPERTY_TYPE" => "F",
                "IBLOCK_ID" => $idIblock
            );
            $ibp = new CIBlockProperty;
            $PropID = $ibp->Add($arFields);
        }
    }
}
?>
```
